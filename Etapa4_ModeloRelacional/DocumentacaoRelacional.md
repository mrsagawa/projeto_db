# Etapa 4 - Projeto Lógico do Banco de Dados usando o Modelo Relacional

## 1. Introdução

Este documento apresenta o projeto lógico do banco de dados para o Sistema de Gestão e Planejamento de Transporte Público (SGPTP) utilizando o Modelo Relacional. Este modelo foi derivado a partir do esquema conceitual EER desenvolvido na etapa anterior, aplicando as regras de mapeamento apropriadas para cada estrutura.
## 2. Esquema Relacional

Abaixo está a proposta de esquema relacional completo, em um único arquivo, contemplando as tabelas, atributos, chaves primárias, chaves estrangeiras e restrições de integridade. Em seguida, há uma breve Análise de Normalização (até a 3ª Forma Normal) para cada tabela.

---

### 2.1 Tabela: LINHA

LINHA (
    id_linha                INTEGER        PRIMARY KEY,
    nome                    VARCHAR(50)    NOT NULL,
    tipo                    VARCHAR(20)    NOT NULL,
    status                  VARCHAR(15)    NOT NULL
        CHECK (status IN ('ativa', 'desativada', 'em_manutencao')),
    data_inauguracao        DATE,
    extensao_total          DECIMAL(10,2)  NOT NULL,
    tempo_medio_percurso    INTEGER        NOT NULL,
    capacidade_passageiros_hora INTEGER    NOT NULL,
    tarifa                  DECIMAL(6,2)   NOT NULL
);

**Observações**:  
- `id_linha` é a chave primária.  
- A coluna `status` tem um CHECK para restringir valores possíveis.  
- `extensao_total`, `tempo_medio_percurso`, `tarifa` etc. são apenas exemplos de atributos adicionais.  
- Se houver o atributo multivalorado `tipos_pagamento`, será implementado em tabela auxiliar (LINHA_TIPOS_PAGAMENTO).

---

### 2.2 Tabela: LINHA_TIPOS_PAGAMENTO (para atributo multivalorado, se existir)

LINHA_TIPOS_PAGAMENTO (
    id_linha       INTEGER      NOT NULL,
    tipo_pagamento VARCHAR(30)  NOT NULL,
    PRIMARY KEY (id_linha, tipo_pagamento),
    FOREIGN KEY (id_linha) REFERENCES LINHA (id_linha)
);

- Cada registro relaciona uma LINHA a uma forma de pagamento aceita (por exemplo: Dinheiro, Cartão etc.).

---

### 2.3 Tabela: ROTA

ROTA (
    id_rota        INTEGER       PRIMARY KEY,
    id_linha       INTEGER       NOT NULL,
    texto_letreiro VARCHAR(50),
    FOREIGN KEY (id_linha) REFERENCES LINHA (id_linha)
);

- Relacionamento “COMPÕE”: 1:N. Uma LINHA pode ter várias ROTAS, mas cada ROTA pertence a exatamente uma LINHA.

---

### 2.4 Tabela: TRECHO

TRECHO (
    id_trecho   INTEGER       PRIMARY KEY,
    comprimento DECIMAL(10,2),
    tempo_medio INTEGER,
    geometria   VARCHAR(2000) -- ou tipo espacial, se disponível
);

- Armazena informações de um segmento de percurso.

---

### 2.5 Tabela: ROTA_TRECHO (para o relacionamento M:N entre ROTA e TRECHO)

ROTA_TRECHO (
    id_rota   INTEGER NOT NULL,
    id_trecho INTEGER NOT NULL,
    ordem     INTEGER,         -- se quiser indicar a sequência do trecho na rota
    PRIMARY KEY (id_rota, id_trecho),
    FOREIGN KEY (id_rota)   REFERENCES ROTA(id_rota),
    FOREIGN KEY (id_trecho) REFERENCES TRECHO(id_trecho)
);

- Uma ROTA pode ser composta de vários TRECHOS, e um TRECHO pode pertencer a várias ROTAS.

---

### 2.6 Tabela: PONTO

PONTO (
    id_ponto  INTEGER      PRIMARY KEY,
    nome      VARCHAR(100),
    latitude  DECIMAL(8,5),
    longitude DECIMAL(8,5)
);

- Representa locais de parada.  
- Se for necessário mapear a relação com TRECHO, pode existir uma tabela TRECHO_PONTO ou atributos em TRECHO apontando para “ponto_inicial” e “ponto_final”.

---

### 2.7 Tabela: TRECHO_PONTO (opcional, se for M:N entre TRECHO e PONTO)

TRECHO_PONTO (
    id_trecho INTEGER NOT NULL,
    id_ponto  INTEGER NOT NULL,
    PRIMARY KEY (id_trecho, id_ponto),
    FOREIGN KEY (id_trecho) REFERENCES TRECHO (id_trecho),
    FOREIGN KEY (id_ponto)  REFERENCES PONTO  (id_ponto)
);

---

### 2.8 Tabela: PARTIDA_PREVISTA

PARTIDA_PREVISTA (
    id_partida      INTEGER     PRIMARY KEY,
    id_rota         INTEGER     NOT NULL,
    dia_semana      VARCHAR(20),
    horario_previsto TIME       NOT NULL,
    FOREIGN KEY (id_rota) REFERENCES ROTA (id_rota)
);

- Relacionamento com ROTA (N:1). Várias partidas podem pertencer à mesma ROTA.

---

### 2.9 Tabela: VIAGEM

VIAGEM (
    id_viagem       INTEGER    PRIMARY KEY,
    id_partida      INTEGER    NOT NULL,
    data_hora       TIMESTAMP  NOT NULL,
    cpf_motorista   CHAR(11)   NOT NULL,
    FOREIGN KEY (id_partida)    REFERENCES PARTIDA_PREVISTA (id_partida),
    FOREIGN KEY (cpf_motorista) REFERENCES MOTORISTA (cpf_motorista)
);

- Relacionamentos: “CUMPRE” (com PARTIDA_PREVISTA), “FAZ” (com MOTORISTA).  
- Guarda o registro de uma viagem efetivamente realizada.

---

### 2.10 Tabela: OCORRENCIA

OCORRENCIA (
    id_ocorrencia  INTEGER     PRIMARY KEY,
    id_viagem      INTEGER     NOT NULL,
    data_hora      TIMESTAMP   NOT NULL,
    descricao      VARCHAR(200),
    tipo           VARCHAR(50),
    FOREIGN KEY (id_viagem) REFERENCES VIAGEM (id_viagem)
);

- Uma VIAGEM pode ter várias ocorrências.

---

### 2.11 Tabela: USUARIO

USUARIO (
    cpf             CHAR(11)     PRIMARY KEY,
    nome            VARCHAR(100) NOT NULL,
    data_nascimento DATE,
    genero          VARCHAR(20)
);

- Representa um passageiro/cliente.

---

### 2.12 Tabela: BILHETE

BILHETE (
    id_bilhete  INTEGER      PRIMARY KEY,
    cpf_usuario CHAR(11)     NOT NULL,
    saldo       DECIMAL(8,2) NOT NULL,
    tipo_cartao VARCHAR(50)  NOT NULL,
    FOREIGN KEY (cpf_usuario) REFERENCES USUARIO(cpf)
);

- Relacionamento “TEM” (1:N) com USUARIO.

---

### 2.13 Tabela: ENTRADA

ENTRADA (
    id_entrada    INTEGER      PRIMARY KEY,
    id_bilhete    INTEGER      NOT NULL,
    cpf_cobrador  CHAR(11),
    tarifa        DECIMAL(6,2) NOT NULL,
    horario       TIMESTAMP    NOT NULL,
    FOREIGN KEY (id_bilhete)   REFERENCES BILHETE (id_bilhete),
    FOREIGN KEY (cpf_cobrador) REFERENCES COBRADOR (cpf_cobrador)
);

- Relacionamento “É USADO EM” (com BILHETE). Se COBRADOR for opcional, `cpf_cobrador` pode ser NULL.

---

### 2.14 Tabela: EMPRESA

EMPRESA (
    cnpj  CHAR(14)      PRIMARY KEY,
    nome  VARCHAR(100)  NOT NULL
);

- Relacionamento “CONTRATA” (1:N) com FUNCIONARIO.

---

### 2.15 Tabela: FUNCIONARIO (superclasse)

FUNCIONARIO (
    cpf            CHAR(11)     PRIMARY KEY,
    cnpj_empresa   CHAR(14)     NOT NULL,
    nome           VARCHAR(100) NOT NULL,
    salario        DECIMAL(10,2),
    FOREIGN KEY (cnpj_empresa) REFERENCES EMPRESA(cnpj)
);

- Todos os funcionários se encontram aqui; as subclasses terão tabelas próprias (Class Table Inheritance) ou uso de uma coluna discriminadora.

---

### 2.16 Tabela: OPERADOR (subclasse de FUNCIONARIO)

OPERADOR (
    cpf_operador CHAR(11)   PRIMARY KEY,
    FOREIGN KEY (cpf_operador) REFERENCES FUNCIONARIO (cpf)
);

- Não adiciona novos atributos além dos herdados.

---

### 2.17 Tabela: MOTORISTA (subclasse de OPERADOR)

MOTORISTA (
    cpf_motorista   CHAR(11)   PRIMARY KEY,
    cod_habilitacao VARCHAR(20),
    FOREIGN KEY (cpf_motorista) REFERENCES OPERADOR (cpf_operador)
);

- Subtipo de OPERADOR, portanto sub-subtipo de FUNCIONARIO.

---

### 2.18 Tabela: COBRADOR (subclasse de OPERADOR)

COBRADOR (
    cpf_cobrador CHAR(11) PRIMARY KEY,
    FOREIGN KEY (cpf_cobrador) REFERENCES OPERADOR (cpf_operador)
);

- Subtipo de OPERADOR.

---

### 2.19 Tabela: TECNICO (subclasse de FUNCIONARIO)

TECNICO (
    cpf_tecnico CHAR(11) PRIMARY KEY,
    FOREIGN KEY (cpf_tecnico) REFERENCES FUNCIONARIO (cpf)
);

- Subtipo de FUNCIONARIO (disjoint de OPERADOR).

---

### 2.20 Tabela: MANUTENCAO

MANUTENCAO (
    id_servico  INTEGER      PRIMARY KEY,
    data        DATE         NOT NULL,
    descricao   VARCHAR(200),
    latitude    DECIMAL(8,5),
    longitude   DECIMAL(8,5),
    cod_veiculo INTEGER,
    FOREIGN KEY (cod_veiculo) REFERENCES VEICULO(cod_veiculo)
);

- Possível relacionamento com VEICULO.  

#### Tabela associativa para TÉCNICO e MANUTENCAO (M:N)

TECNICO_MANUTENCAO (
    cpf_tecnico CHAR(11)  NOT NULL,
    id_servico  INTEGER   NOT NULL,
    PRIMARY KEY (cpf_tecnico, id_servico),
    FOREIGN KEY (cpf_tecnico) REFERENCES TECNICO (cpf_tecnico),
    FOREIGN KEY (id_servico)  REFERENCES MANUTENCAO (id_servico)
);

---

### 2.21 Tabela: VEICULO

VEICULO (
    cod_veiculo          INTEGER      PRIMARY KEY,
    placa                VARCHAR(10)  NOT NULL UNIQUE,
    data_inicio_operacao DATE,
    latitude             DECIMAL(8,5),
    longitude            DECIMAL(8,5),
    nome_modelo          VARCHAR(50)  NOT NULL,
    id_garagem           INTEGER      NOT NULL,
    FOREIGN KEY (nome_modelo) REFERENCES MODELO (nome_modelo),
    FOREIGN KEY (id_garagem) REFERENCES GARAGEM (id_garagem)
);

- Relacionamento com MODELO (N:1, “POSSUI”) e com GARAGEM (N:1, “ESTACIONA EM”).

---

### 2.22 Tabela: MODELO

MODELO (
    nome_modelo VARCHAR(50) PRIMARY KEY,
    tipo        VARCHAR(50),
    fabricante  VARCHAR(100),
    capacidade  INTEGER
);

---

### 2.23 Tabela: GARAGEM

GARAGEM (
    id_garagem     INTEGER       PRIMARY KEY,
    estoque_diesel DECIMAL(10,2),
    num_eletropostos INTEGER,
    capacidade     INTEGER,
    logradouro     VARCHAR(100),
    numero         VARCHAR(10),
    cidade         VARCHAR(50),
    cep            VARCHAR(10)
);

---

### 2.24 Tabela: ESCALA

ESCALA (
    id_escala    INTEGER      PRIMARY KEY,
    hora_inicio  TIME         NOT NULL,
    hora_fim     TIME         NOT NULL
);

#### Tabela associativa: ESCALA_FUNCIONARIO (relacionamento M:N, “É CUMPRIDA POR”)

ESCALA_FUNCIONARIO (
    id_escala  INTEGER   NOT NULL,
    cpf_func   CHAR(11)  NOT NULL,
    PRIMARY KEY (id_escala, cpf_func),
    FOREIGN KEY (id_escala) REFERENCES ESCALA (id_escala),
    FOREIGN KEY (cpf_func)  REFERENCES FUNCIONARIO (cpf)
);

---

## 3. Análise de Normalização

A maior parte das tabelas propostas estão em **3ª Forma Normal (3FN)**, pois:

1. **1FN**:  
   - Os atributos são atômicos e as repetições multivaloradas foram isoladas (ex.: LINHA_TIPOS_PAGAMENTO).

2. **2FN**:  
   - Não há dependências parciais em PKs simples. Quando há PK composta (em tabelas associativas), os atributos não-chave (se houver) dependem do conjunto inteiro de atributos da PK.

3. **3FN**:  
   - Não existem atributos não-chave que dependam de outros atributos não-chave, evitando dependências transitivas.

### Exemplos rápidos de verificação:

- **Tabela LINHA**:  
  - PK = `id_linha`. Todos os outros atributos dependem somente de `id_linha`. Sem dependências transitivas.

- **Tabela ROTA**:  
  - PK = `id_rota`, FK para `id_linha`. Nenhum atributo depende de outro atributo não-chave.

- **Tabela ROTA_TRECHO** (PK composta):  
  - PK = (`id_rota`, `id_trecho`). O atributo `ordem` depende do par inteiro. Não há dependência parcial ou transitiva.

- **Tabela FUNCIONARIO** + Subclasses (OPERADOR, TÉCNICO, MOTORISTA, COBRADOR)**:  
  - Em “Class Table Inheritance”, cada subclasse tem como PK a PK herdada. Não há violação de 3FN.

- **Tabela VEICULO**:  
  - PK = `cod_veiculo`. FKs = `nome_modelo`, `id_garagem`. Todos os atributos não-chave dependem de `cod_veiculo`.

- **Tabela ESCALA_FUNCIONARIO** (PK composta):  
  - Atributos não-chave (se existirem) dependeriam do par (`id_escala`, `cpf_func`).  
  - Em 3FN.

Como não há indicativos de violações formais (colunas calculadas, dependências “entre” colunas não-chave etc.), podemos afirmar que as tabelas seguem adequadamente a 3FN. Caso haja regras de negócio adicionais, podem ser tratadas por CHECK constraints, triggers ou pela aplicação.

---

#### Forma Normal de Boyce-Codd (FNBC)
- Todas as tabelas estão na 3FN.
- Para cada dependência funcional X → Y, X é uma superchave na tabela.
- Não existem anomalias de dependências funcionais nas tabelas.

### 3.2 Conclusão da Análise de Normalização

O esquema relacional proposto está na Forma Normal de Boyce-Codd (FNBC), pois:

1. Todas as tabelas estão na 3FN.
2. As chaves candidatas foram identificadas e definidas corretamente.
3. Para cada dependência funcional, o determinante é uma superchave.
4. As relações N:M foram corretamente transformadas em tabelas de junção.
5. Atributos multivalorados foram corretamente normalizados em tabelas separadas.
6. As especializações foram implementadas usando o método de tabelas separadas com chaves estrangeiras.

Como o esquema já está na FNBC, não é necessário realizar um processo adicional de normalização.

## 4. Índices Propostos

Para otimizar o desempenho das consultas mais frequentes, propomos a criação dos seguintes índices:

1. Índices em todas as chaves estrangeiras para melhorar a performance das junções.
2. Índice em LINHA(nome) para consultas por nome/número de linha.
3. Índice em ESTACAO(nome) para consultas por nome de estação.
4. Índice em ESTACAO(latitude, longitude) para consultas geográficas.
5. Índice em VEICULO(placa) para consultas por placa de veículo.
6. Índice em OPERADOR(nome) para buscas por nome de operador.
7. Índice em VIAGEM(horario_prog_partida) para consultas por horário.
8. Índice em DEMANDA(data, faixa_horaria) para análises de demanda.
9. Índice em MANUTENCAO(data_inicio) para consultas de agendamento.
10. Índice em BILHETE(data_hora_emissao) para análises de vendas.

## 5. Gatilhos (Triggers) e Procedimentos Armazenados

Sugerimos a implementação dos seguintes gatilhos e procedimentos armazenados para garantir a integridade dos dados e automatizar operações comuns:

### 5.1 Gatilhos (Triggers)

1. **verificaCapacidadeVeiculo**: Verifica se o veículo alocado a uma viagem tem capacidade compatível com a demanda esperada.
2. **atualizaEstadoVeiculo**: Atualiza automaticamente o estado do veículo para "em_manutencao" quando uma nova manutenção for registrada.
3. **validaHorarioViagem**: Verifica se os horários de uma nova viagem não conflitam com outras viagens do mesmo veículo.
4. **verificaDisponibilidadeOperador**: Verifica se o operador está disponível no horário da viagem a ser alocada.
5. **atualizaQuilometragem**: Atualiza a quilometragem do veículo após a conclusão de uma viagem.
6. **calculaTempoPercurso**: Calcula e atualiza o tempo real de percurso após a conclusão de uma viagem.
7. **verificaManutencoesPreventivas**: Identifica veículos que necessitam de manutenção preventiva com base na quilometragem.

### 5.2 Procedimentos Armazenados

1. **gerarEscalaOperadores(data)**: Gera uma escala otimizada de operadores para uma determinada data.
2. **calcularDemandaProjetada(id_linha, data)**: Projeta a demanda para uma linha em uma data específica com base em histórico.
3. **calcularRota(origem, destino)**: Encontra a melhor rota entre duas estações.
4. **gerarRelatorioOcorrencias(periodo)**: Gera um relatório estatístico de ocorrências por período.
5. **analisarEficienciaLinha(id_linha)**: Analisa indicadores de eficiência operacional de uma linha.
6. **simularNovaLinha(parametros)**: Simula o impacto da criação de uma nova linha.
7. **gerarRelatorioManutencoes(periodo)**: Gera um relatório de manutenções realizadas em um período.
8. **calcularTaxaOcupacao(id_linha, data, hora)**: Calcula a taxa de ocupação para uma linha em determinado horário.

## 6. Visões (Views)

Para facilitar consultas complexas e recorrentes, propomos a criação das seguintes visões:

1. **ViagemCompleta**: Combina informações de viagem, rota, linha, veículo e operador.
2. **OcorrenciaDetalhada**: Informações detalhadas sobre ocorrências incluindo viagem e linha associadas.
3. **ManutencaoVeiculo**: Histórico de manutenções por veículo.
4. **DemandaConsolidadaLinha**: Demanda consolidada por linha, período e tipo de dia.
5. **EstacoesComServiços**: Informações de estações incluindo todos os serviços disponíveis.
6. **OperadorQualificacao**: Informações de operadores incluindo suas qualificações.
7. **VeiculoEquipamento**: Informações de veículos incluindo seus equipamentos.
8. **ProjetosEmAndamento**: Projetos de expansão ativos com suas simulações associadas.
9. **RotaCompleta**: Informações completas de rota incluindo todas as estações na ordem correta.
10. **BilacoOperacional**: Resumo operacional diário com viagens realizadas, passageiros transportados e ocorrências.

## 7. Conclusão

O projeto lógico apresentado neste documento seguiu as regras de mapeamento do modelo EER para o modelo Relacional, garantindo que todas as abstrações e restrições semânticas do domínio fossem preservadas. O esquema está na Forma Normal de Boyce-Codd, garantindo a minimização de redundâncias e anomalias.

Além disso, foram propostos índices, gatilhos, procedimentos armazenados e visões para otimizar o desempenho e facilitar as operações comuns no sistema. O esquema relacional completo está representado no diagrama disponível no arquivo `DiagramaRelacional.png` nesta pasta.
