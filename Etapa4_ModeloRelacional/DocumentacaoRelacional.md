# Etapa 4 - Projeto Lógico do Banco de Dados usando o Modelo Relacional

## 1. Introdução

Este documento apresenta o projeto lógico do banco de dados para o Sistema de Gestão e Planejamento de Transporte Público (SGPTP) utilizando o Modelo Relacional. Este modelo foi derivado a partir do esquema conceitual EER desenvolvido na etapa anterior, aplicando as regras de mapeamento apropriadas para cada estrutura.

## 2. Esquema Relacional

Abaixo está descrito o esquema relacional completo, incluindo todas as tabelas, atributos, chaves primárias, chaves estrangeiras e restrições de integridade.

### 2.1 Tabela: LINHA

```
LINHA (
    id_linha INTEGER PRIMARY KEY,
    nome VARCHAR(50) NOT NULL,
    tipo VARCHAR(20) NOT NULL,
    status VARCHAR(15) CHECK (status IN ('ativa', 'desativada', 'em_manutencao')),
    data_inauguracao DATE,
    extensao_total DECIMAL(10,2) NOT NULL,
    tempo_medio_percurso INTEGER NOT NULL,
    capacidade_passageiros_hora INTEGER NOT NULL,
    tarifa DECIMAL(6,2) NOT NULL
)
```

### 2.2 Tabela: ESTACAO

```
ESTACAO (
    id_estacao INTEGER PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    tipo VARCHAR(20) NOT NULL,
    status VARCHAR(15) CHECK (status IN ('operacional', 'em_manutencao', 'desativada')),
    latitude DECIMAL(10,8) NOT NULL,
    longitude DECIMAL(11,8) NOT NULL,
    endereco VARCHAR(200) NOT NULL,
    acessibilidade BOOLEAN DEFAULT FALSE,
    capacidade INTEGER NOT NULL,
    horario_abertura TIME NOT NULL,
    horario_fechamento TIME NOT NULL
)
```

### 2.3 Tabela: TERMINAL (Especialização de ESTACAO)

```
TERMINAL (
    id_estacao INTEGER PRIMARY KEY,
    num_plataformas INTEGER NOT NULL,
    area_total DECIMAL(10,2) NOT NULL,
    servicos_disponiveis TEXT,
    FOREIGN KEY (id_estacao) REFERENCES ESTACAO(id_estacao) ON DELETE CASCADE
)
```

### 2.4 Tabela: PONTO_INTERMEDIARIO (Especialização de ESTACAO)

```
PONTO_INTERMEDIARIO (
    id_estacao INTEGER PRIMARY KEY,
    tipo_abrigo VARCHAR(20) NOT NULL,
    sinalizacao VARCHAR(50),
    FOREIGN KEY (id_estacao) REFERENCES ESTACAO(id_estacao) ON DELETE CASCADE
)
```

### 2.5 Tabela: ESTACAO_SERVICO (Atributo multivalorado de ESTACAO)

```
ESTACAO_SERVICO (
    id_estacao INTEGER,
    servico VARCHAR(50) NOT NULL,
    PRIMARY KEY (id_estacao, servico),
    FOREIGN KEY (id_estacao) REFERENCES ESTACAO(id_estacao) ON DELETE CASCADE
)
```

### 2.6 Tabela: TRECHO

```
TRECHO (
    id_trecho INTEGER PRIMARY KEY,
    estacao_origem INTEGER NOT NULL,
    estacao_destino INTEGER NOT NULL,
    distancia DECIMAL(8,2) NOT NULL,
    tempo_medio INTEGER NOT NULL,
    estado_conservacao VARCHAR(20) NOT NULL,
    data_ultima_manutencao DATE,
    tipo_via VARCHAR(30) NOT NULL,
    FOREIGN KEY (estacao_origem) REFERENCES ESTACAO(id_estacao),
    FOREIGN KEY (estacao_destino) REFERENCES ESTACAO(id_estacao),
    CHECK (estacao_origem != estacao_destino)
)
```

### 2.7 Tabela: ROTA

```
ROTA (
    id_rota INTEGER PRIMARY KEY,
    id_linha INTEGER NOT NULL,
    direcao VARCHAR(10) CHECK (direcao IN ('ida', 'volta')),
    horario_inicio_operacao TIME NOT NULL,
    horario_fim_operacao TIME NOT NULL,
    frequencia INTEGER NOT NULL,
    dias_operacao VARCHAR(50) NOT NULL,
    FOREIGN KEY (id_linha) REFERENCES LINHA(id_linha) ON DELETE CASCADE
)
```

### 2.8 Tabela: ROTA_ESTACAO (Relação N:M entre ROTA e ESTACAO)

```
ROTA_ESTACAO (
    id_rota INTEGER,
    id_estacao INTEGER,
    ordem INTEGER NOT NULL,
    tempo_parada INTEGER NOT NULL,
    PRIMARY KEY (id_rota, id_estacao),
    FOREIGN KEY (id_rota) REFERENCES ROTA(id_rota) ON DELETE CASCADE,
    FOREIGN KEY (id_estacao) REFERENCES ESTACAO(id_estacao) ON DELETE CASCADE
)
```

### 2.9 Tabela: VEICULO

```
VEICULO (
    id_veiculo INTEGER PRIMARY KEY,
    tipo VARCHAR(20) NOT NULL,
    modelo VARCHAR(50) NOT NULL,
    placa VARCHAR(10) UNIQUE NOT NULL,
    capacidade INTEGER NOT NULL,
    ano_fabricacao INTEGER NOT NULL,
    data_aquisicao DATE NOT NULL,
    estado VARCHAR(15) CHECK (estado IN ('ativo', 'em_manutencao', 'desativado')),
    quilometragem INTEGER NOT NULL,
    tipo_combustivel VARCHAR(20) NOT NULL,
    acessibilidade BOOLEAN DEFAULT FALSE
)
```

### 2.10 Tabela: VEICULO_EQUIPAMENTO (Atributo multivalorado de VEICULO)

```
VEICULO_EQUIPAMENTO (
    id_veiculo INTEGER,
    equipamento VARCHAR(50) NOT NULL,
    PRIMARY KEY (id_veiculo, equipamento),
    FOREIGN KEY (id_veiculo) REFERENCES VEICULO(id_veiculo) ON DELETE CASCADE
)
```

### 2.11 Tabela: ONIBUS (Especialização de VEICULO)

```
ONIBUS (
    id_veiculo INTEGER PRIMARY KEY,
    comprimento DECIMAL(5,2) NOT NULL,
    num_portas INTEGER NOT NULL,
    articulado BOOLEAN DEFAULT FALSE,
    FOREIGN KEY (id_veiculo) REFERENCES VEICULO(id_veiculo) ON DELETE CASCADE
)
```

### 2.12 Tabela: VAGAO (Especialização de VEICULO)

```
VAGAO (
    id_veiculo INTEGER PRIMARY KEY,
    potencia INTEGER NOT NULL,
    sistema_tracao VARCHAR(30) NOT NULL,
    capacidade_pe INTEGER NOT NULL,
    capacidade_sentado INTEGER NOT NULL,
    FOREIGN KEY (id_veiculo) REFERENCES VEICULO(id_veiculo) ON DELETE CASCADE
)
```

### 2.13 Tabela: OPERADOR

```
OPERADOR (
    id_operador INTEGER PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    cargo VARCHAR(30) NOT NULL,
    registro VARCHAR(20) UNIQUE NOT NULL,
    data_admissao DATE NOT NULL,
    status VARCHAR(15) CHECK (status IN ('ativo', 'afastado', 'inativo')),
    telefone VARCHAR(20) NOT NULL,
    email VARCHAR(50),
    endereco VARCHAR(200) NOT NULL
)
```

### 2.14 Tabela: OPERADOR_QUALIFICACAO (Atributo multivalorado de OPERADOR)

```
OPERADOR_QUALIFICACAO (
    id_operador INTEGER,
    qualificacao VARCHAR(50) NOT NULL,
    PRIMARY KEY (id_operador, qualificacao),
    FOREIGN KEY (id_operador) REFERENCES OPERADOR(id_operador) ON DELETE CASCADE
)
```

### 2.15 Tabela: MOTORISTA (Especialização de OPERADOR)

```
MOTORISTA (
    id_operador INTEGER PRIMARY KEY,
    num_cnh VARCHAR(15) UNIQUE NOT NULL,
    categoria_cnh VARCHAR(5) NOT NULL,
    validade_cnh DATE NOT NULL,
    FOREIGN KEY (id_operador) REFERENCES OPERADOR(id_operador) ON DELETE CASCADE
)
```

### 2.16 Tabela: CONDUTOR_TREM (Especialização de OPERADOR)

```
CONDUTOR_TREM (
    id_operador INTEGER PRIMARY KEY,
    certificacao VARCHAR(30) NOT NULL,
    horas_treinamento INTEGER NOT NULL,
    FOREIGN KEY (id_operador) REFERENCES OPERADOR(id_operador) ON DELETE CASCADE
)
```

### 2.17 Tabela: MANUTENCAO

```
MANUTENCAO (
    id_manutencao INTEGER PRIMARY KEY,
    tipo VARCHAR(20) CHECK (tipo IN ('preventiva', 'corretiva')),
    data_inicio DATETIME NOT NULL,
    data_conclusao DATETIME,
    descricao TEXT NOT NULL,
    custo DECIMAL(10,2) NOT NULL,
    responsavel VARCHAR(100) NOT NULL,
    status VARCHAR(15) CHECK (status IN ('agendada', 'em_andamento', 'concluida', 'cancelada')),
    id_veiculo INTEGER,
    id_estacao INTEGER,
    CHECK ((id_veiculo IS NOT NULL AND id_estacao IS NULL) OR (id_veiculo IS NULL AND id_estacao IS NOT NULL)),
    FOREIGN KEY (id_veiculo) REFERENCES VEICULO(id_veiculo),
    FOREIGN KEY (id_estacao) REFERENCES ESTACAO(id_estacao)
)
```

### 2.18 Tabela: MANUTENCAO_PECA (Peças utilizadas em uma manutenção)

```
MANUTENCAO_PECA (
    id_manutencao INTEGER,
    peca VARCHAR(50) NOT NULL,
    quantidade INTEGER NOT NULL,
    PRIMARY KEY (id_manutencao, peca),
    FOREIGN KEY (id_manutencao) REFERENCES MANUTENCAO(id_manutencao) ON DELETE CASCADE
)
```

### 2.19 Tabela: VIAGEM

```
VIAGEM (
    id_viagem INTEGER PRIMARY KEY,
    id_rota INTEGER NOT NULL,
    id_veiculo INTEGER NOT NULL,
    id_operador INTEGER NOT NULL,
    horario_prog_partida DATETIME NOT NULL,
    horario_real_partida DATETIME,
    horario_prog_chegada DATETIME NOT NULL,
    horario_real_chegada DATETIME,
    status VARCHAR(15) CHECK (status IN ('programada', 'em_andamento', 'concluida', 'cancelada')),
    num_passageiros INTEGER,
    FOREIGN KEY (id_rota) REFERENCES ROTA(id_rota),
    FOREIGN KEY (id_veiculo) REFERENCES VEICULO(id_veiculo),
    FOREIGN KEY (id_operador) REFERENCES OPERADOR(id_operador)
)
```

### 2.20 Tabela: OCORRENCIA

```
OCORRENCIA (
    id_ocorrencia INTEGER PRIMARY KEY,
    id_viagem INTEGER NOT NULL,
    tipo VARCHAR(30) NOT NULL,
    data_hora DATETIME NOT NULL,
    localizacao VARCHAR(200) NOT NULL,
    descricao TEXT NOT NULL,
    gravidade VARCHAR(15) CHECK (gravidade IN ('baixa', 'media', 'alta', 'critica')),
    solucao TEXT,
    tempo_resolucao INTEGER,
    impacto TEXT,
    FOREIGN KEY (id_viagem) REFERENCES VIAGEM(id_viagem) ON DELETE CASCADE
)
```

### 2.21 Tabela: DEMANDA

```
DEMANDA (
    id_demanda INTEGER PRIMARY KEY,
    data DATE NOT NULL,
    faixa_horaria VARCHAR(20) NOT NULL,
    volume_passageiros INTEGER NOT NULL,
    tipo_dia VARCHAR(10) CHECK (tipo_dia IN ('util', 'sabado', 'domingo', 'feriado')),
    taxa_ocupacao DECIMAL(5,2) NOT NULL,
    origem_dados VARCHAR(50) NOT NULL,
    id_linha INTEGER,
    id_estacao INTEGER,
    CHECK ((id_linha IS NOT NULL AND id_estacao IS NULL) OR (id_linha IS NULL AND id_estacao IS NOT NULL)),
    FOREIGN KEY (id_linha) REFERENCES LINHA(id_linha),
    FOREIGN KEY (id_estacao) REFERENCES ESTACAO(id_estacao)
)
```

### 2.22 Tabela: BILHETE

```
BILHETE (
    id_bilhete INTEGER PRIMARY KEY,
    tipo VARCHAR(20) NOT NULL,
    data_hora_emissao DATETIME NOT NULL,
    valor DECIMAL(6,2) NOT NULL,
    id_usuario VARCHAR(50)
)
```

### 2.23 Tabela: BILHETE_VIAGEM (Relação N:M entre BILHETE e VIAGEM)

```
BILHETE_VIAGEM (
    id_bilhete INTEGER,
    id_viagem INTEGER,
    timestamp_utilizacao DATETIME NOT NULL,
    estacao_entrada INTEGER NOT NULL,
    estacao_saida INTEGER,
    PRIMARY KEY (id_bilhete, id_viagem),
    FOREIGN KEY (id_bilhete) REFERENCES BILHETE(id_bilhete),
    FOREIGN KEY (id_viagem) REFERENCES VIAGEM(id_viagem),
    FOREIGN KEY (estacao_entrada) REFERENCES ESTACAO(id_estacao),
    FOREIGN KEY (estacao_saida) REFERENCES ESTACAO(id_estacao)
)
```

### 2.24 Tabela: PROJETO_EXPANSAO

```
PROJETO_EXPANSAO (
    id_projeto INTEGER PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    descricao TEXT NOT NULL,
    status VARCHAR(20) CHECK (status IN ('em_analise', 'aprovado', 'em_execucao', 'concluido', 'cancelado')),
    data_inicio_prev DATE NOT NULL,
    data_conclusao_prev DATE NOT NULL,
    orcamento DECIMAL(15,2) NOT NULL,
    regiao VARCHAR(100) NOT NULL,
    beneficios TEXT NOT NULL,
    responsavel VARCHAR(100) NOT NULL
)
```

### 2.25 Tabela: PROJETO_LINHA (Relação entre PROJETO_EXPANSAO e novas LINHAS)

```
PROJETO_LINHA (
    id_projeto INTEGER,
    id_linha INTEGER,
    impacto_estimado TEXT NOT NULL,
    prioridade INTEGER NOT NULL,
    PRIMARY KEY (id_projeto, id_linha),
    FOREIGN KEY (id_projeto) REFERENCES PROJETO_EXPANSAO(id_projeto),
    FOREIGN KEY (id_linha) REFERENCES LINHA(id_linha)
)
```

### 2.26 Tabela: SIMULACAO

```
SIMULACAO (
    id_simulacao INTEGER PRIMARY KEY,
    id_projeto INTEGER NOT NULL,
    data_criacao DATETIME NOT NULL,
    parametros TEXT NOT NULL,
    resultados TEXT NOT NULL,
    demanda_projetada INTEGER NOT NULL,
    custo_operacional DECIMAL(15,2) NOT NULL,
    impacto_rede TEXT NOT NULL,
    validacao VARCHAR(10) CHECK (validacao IN ('aprovada', 'rejeitada', 'pendente')),
    FOREIGN KEY (id_projeto) REFERENCES PROJETO_EXPANSAO(id_projeto)
)
```

### 2.27 Tabela: SIMULACAO_CENARIO (Para os diferentes cenários de uma simulação)

```
SIMULACAO_CENARIO (
    id_simulacao INTEGER,
    cenario VARCHAR(50) NOT NULL,
    confiabilidade DECIMAL(5,2) NOT NULL,
    descricao TEXT NOT NULL,
    PRIMARY KEY (id_simulacao, cenario),
    FOREIGN KEY (id_simulacao) REFERENCES SIMULACAO(id_simulacao) ON DELETE CASCADE
)
```

## 3. Análise de Normalização

### 3.1 Verificação das Formas Normais

#### Primeira Forma Normal (1FN)
- Todas as tabelas possuem chaves primárias definidas.
- Todos os atributos são atômicos (não-divisíveis).
- Atributos multivalorados foram transformados em tabelas separadas (ex: ESTACAO_SERVICO, VEICULO_EQUIPAMENTO, OPERADOR_QUALIFICACAO).
- Atributos compostos foram decompostos (ex: coordenadas geográficas em latitude e longitude, contatos separados em telefone, email e endereço).

#### Segunda Forma Normal (2FN)
- Todas as tabelas estão na 1FN.
- Todos os atributos não-chave dependem da chave primária completa.
- Nas tabelas com chaves compostas (ex: ROTA_ESTACAO, BILHETE_VIAGEM), todos os atributos dependem da combinação completa das chaves.

#### Terceira Forma Normal (3FN)
- Todas as tabelas estão na 2FN.
- Não existem dependências transitivas, ou seja, nenhum atributo não-chave depende de outro atributo não-chave.
- As informações relacionadas a entidades distintas foram separadas em tabelas próprias (ex: VEICULO, OPERADOR, MANUTENCAO).

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
