# Etapa 3 - Projeto Conceitual do Banco de Dados usando o Modelo EER

## 1. Introdução

Este documento apresenta o projeto conceitual do banco de dados para o Sistema de Gestão e Planejamento de Transporte Público (SGPTP) utilizando o Modelo Entidade-Relacionamento Estendido (EER). O modelo EER permite representar com maior precisão as abstrações e restrições semânticas identificadas durante a análise de requisitos.

## 2. Diagrama EER

TODO!

## 3. Entidades e Seus Atributos

## 3. Entidades e Seus Atributos

### 3.1 Entidade: LINHA

#### Descrição
Representa as linhas de transporte público disponíveis na rede.

#### Atributos
- **cod_linha** (Chave primária): Identificador único da linha.
- **nome_linha**: Nome ou número da linha.
- **data_criacao**: Data em que a linha começou a operar.


### 3.2 Entidade: ROTA

#### Descrição
Representa o trajeto específico dentro de uma linha, que pode incluir pontos, trechos e um itinerário definido.

#### Atributos
- **cod_rota** (Chave primária): Identificador único da rota.
- **texto_letreiro**: Texto exibido no letreiro, indicando o destino ou nome da rota.

### 3.3 Entidade: TRECHO

#### Descrição
Define um segmento específico de rota entre dois ou mais pontos.

#### Atributos
- **cod_trecho** (Chave primária, se for necessário identificar unicamente cada trecho): Identificador único do trecho.
- **comprimento**: Extensão do trecho (em km ou metros).
- **tempo_medio**: Tempo médio de percurso.
- **geometria**: Dados espaciais ou sequência de coordenadas do trecho.


### 3.4 Entidade: PONTO

#### Descrição
Representa locais de parada ou referência em uma rota, como pontos de ônibus ou cruzamentos.

#### Atributos
- **cod_ponto** (Chave primária): Identificador único do ponto.
- **nome**: Nome descritivo do ponto (ex.: “Terminal Central”).
- **latitude** (opcional): Latitude do ponto.
- **longitude** (opcional): Longitude do ponto.


### 3.5 Entidade: PARTIDA_PREVISTA

#### Descrição
Armazena os horários programados (dia e hora) em que as viagens devem ocorrer.

#### Atributos
- **cod_partida** (Chave primária, se necessário): Identificador único da partida prevista.
- **dia_semana**: Dia da semana (ex.: Segunda, Terça) ou codificação (0–6).
- **horario_previsto**: Horário programado de saída.


### 3.6 Entidade: VIAGEM

#### Descrição
Registra a realização efetiva de uma rota em um horário específico, associada a um motorista e a um veículo.

#### Atributos
- **cod_viagem** (Chave primária): Identificador único da viagem.
- **data_hora**: Momento de início (ou registro) da viagem.


### 3.7 Entidade: OCORRÊNCIA

#### Descrição
Registra incidentes ou eventos que ocorreram durante uma viagem (como atrasos, acidentes, problemas mecânicos etc.).

#### Atributos
- **cod_ocorrencia** (Chave primária): Identificador único da ocorrência.
- **data_hora**: Data e hora do registro do evento.
- **descricao**: Detalhes sobre a ocorrência.
- **tipo**: Classificação da ocorrência (ex.: “pane mecânica”, “acidente”, etc.).


### 3.8 Entidade: USUÁRIO

#### Descrição
Representa o passageiro ou cliente que utiliza o sistema de transporte.

#### Atributos
- **cpf** (Chave primária): CPF do usuário (identificador único).
- **nome**: Nome completo do usuário.
- **data_nascimento**: Data de nascimento do usuário.
- **genero**: Informação de gênero (opcional).


### 3.9 Entidade: BILHETE

#### Descrição
Representa o cartão ou passe utilizado pelo usuário para pagar a tarifa de transporte.

#### Atributos
- **cod_cartao** (Chave primária): Identificador único do bilhete/cartão.
- **saldo**: Saldo monetário atual disponível no cartão.
- **tipo_cartao**: Tipo do bilhete (ex.: Estudante, Comum, Idoso, etc.).


### 3.10 Entidade: ENTRADA

#### Descrição
Registra o ato de entrada do passageiro no veículo ou terminal, incluindo valor da tarifa cobrada e horário.

#### Atributos
- **cod_entrada** (Chave primária, se necessário): Identificador único da entrada.
- **tarifa**: Valor cobrado no momento da entrada.
- **horario**: Momento em que o usuário passou pela catraca ou registrou a entrada.


### 3.11 Entidade: COBRADOR

#### Descrição
Subtipo de Operador responsável por receber pagamento de passagens ou validar bilhetes.

#### Atributos
- *Herdados de OPERADOR*: cpf, nome, salario.
- *Não possui atributos próprios além dos herdados* (conforme o diagrama atual).

### 3.12 Entidade: MOTORISTA

#### Descrição
Subtipo de Operador responsável por conduzir o veículo no percurso definido.

#### Atributos
- *Herdados de OPERADOR*: cpf, nome, salario.
- **cod_habilitacao** (ou num_cnh): Número de registro da habilitação.

### 3.13 Entidade: TÉCNICO

#### Descrição
Subtipo de Funcionário incumbido das atividades de manutenção de veículos.

#### Atributos
- *Herdados de FUNCIONÁRIO*: cpf, nome, salario.
- *Não possui atributos próprios além dos herdados*.


### 3.14 Entidade: OPERADOR

#### Descrição
Subtipo de Funcionário que atua diretamente na operação do sistema, podendo ser motorista ou cobrador.

#### Atributos
- *Herdados de FUNCIONÁRIO*: cpf, nome, salario.
- *Não possui atributos próprios além dos herdados*.


### 3.15 Entidade: FUNCIONÁRIO

#### Descrição
Representa o colaborador vinculado à empresa de transporte, podendo ter diferentes funções.

#### Atributos
- **cpf** (Chave primária): CPF do funcionário.
- **nome**: Nome completo do funcionário.
- **salario**: Valor de salário do funcionário.


### 3.16 Entidade: ESCALA

#### Descrição
Define o horário de trabalho de um funcionário, incluindo hora de início e hora de término.

#### Atributos
- **cod_escala** (Chave primária, se necessário): Identificador único da escala.
- **hora_inicio**: Horário em que a escala começa.
- **hora_fim**: Horário em que a escala termina.


### 3.17 Entidade: EMPRESA

#### Descrição
Representa a companhia que gerencia e contrata os funcionários para o sistema de transporte.

#### Atributos
- **cnpj** (Chave primária): Identificador único da empresa.
- **nome**: Nome legal da empresa.


### 3.18 Entidade: MANUTENÇÃO

#### Descrição
Registra serviços de reparo ou revisão feitos nos veículos.

#### Atributos
- **cod_servico** (Chave primária): Identificador único da manutenção/serviço.
- **data**: Data em que a manutenção ocorreu.
- **descricao**: Detalhes sobre o tipo de manutenção realizada.
- **latitude** (opcional): Localização do serviço (quando aplicável).
- **longitude** (opcional): Localização do serviço (quando aplicável).


### 3.19 Entidade: VEÍCULO

#### Descrição
Representa o veículo utilizado para prestar serviço de transporte, como ônibus ou vans.

#### Atributos
- **cod_veiculo** (Chave primária): Identificador único do veículo.
- **placa**: Placa de identificação.
- **data_inicio_operacao**: Data em que o veículo começou a operar.
- **latitude** (opcional): Localização atual do veículo.
- **longitude** (opcional): Localização atual do veículo.


### 3.20 Entidade: MODELO

#### Descrição
Representa o modelo do veículo, incluindo informações de capacidade e fabricante.

#### Atributos
- **nome** (Chave primária ou não, dependendo da modelagem): Nome do modelo.
- **tipo**: Tipo do veículo (ex.: ônibus urbano, micro-ônibus, elétrico, etc.).
- **fabricante**: Fabricante do veículo.
- **capacidade**: Capacidade de transporte (número de passageiros).


### 3.21 Entidade: GARAGEM

#### Descrição
Local onde os veículos são estacionados, reabastecidos e mantidos.

#### Atributos
- **codigo** (Chave primária): Identificador único da garagem.
- **estoque_diesel**: Quantidade de diesel disponível (quando aplicável).
- **num_eletropostos**: Quantidade de pontos de recarga elétrica.
- **capacidade**: Capacidade máxima de veículos.
- **logradouro**: Nome da rua ou avenida.
- **numero**: Número do endereço.
- **cidade**: Município onde a garagem se localiza.
- **cep**: Código de Endereçamento Postal da localização.


## 4. Relacionamentos

### 4.1 Relacionamento: CONTRATA (entre EMPRESA e FUNCIONÁRIO)
- **Cardinalidade**: 1:N  
  - Uma EMPRESA contrata vários FUNCIONÁRIOS (1 para EMPRESA, N para FUNCIONÁRIO).  
  - Cada FUNCIONÁRIO é contratado por apenas uma EMPRESA.
- **Atributos**: Não possui


### 4.2 Relacionamento: É CUMPRIDA POR (entre ESCALA e FUNCIONÁRIO)
- **Cardinalidade**: M:N <!-- discutir -->  
  - Uma ESCALA pode ser cumprida por vários FUNCIONÁRIOS, e um FUNCIONÁRIO pode cumprir diversas escalas (dependendo da modelagem exata).
- **Atributos**: Não possui


### 4.3 Relacionamento: É FEITA EM (entre TÉCNICO e MANUTENÇÃO)
- **Cardinalidade**: M:N  
  - Um TÉCNICO pode realizar várias MANUTENÇÕES.  
  - Uma MANUTÊNÇÃO pode ser feita por vários TÉCNICOS.
- **Atributos**: Não possui


### 4.4 Relacionamento: POSSUI (entre VEÍCULO e MODELO)
- **Cardinalidade**: N:1  
  - Vários VEÍCULOS podem ter o mesmo MODELO.  
  - Cada VEÍCULO possui exatamente um MODELO.
- **Atributos**: Não possui


### 4.5 Relacionamento: ESTACIONA EM (entre VEÍCULO e GARAGEM)
- **Cardinalidade**: N:1  
  - Vários VEÍCULOS podem estar estacionados em uma GARAGEM.  
  - Cada VEÍCULO se estaciona em uma única GARAGEM.
- **Atributos**: Não possui


### 4.6 Relacionamento: SEGUE (entre VEÍCULO e ROTA)
- **Cardinalidade**: M:N (presumido)  
  - Um VEÍCULO pode seguir diferentes ROTAS ao longo do dia.  
  - Uma ROTA pode ser seguida por vários VEÍCULOS.
- **Atributos**: Não possui


### 4.7 Relacionamento: COMPÕE (entre LINHA e ROTA)
- **Cardinalidade**: 1:N <!-- discutir -->  
  - Uma LINHA pode ter múltiplas ROTAS.  
  - Cada ROTA pertence a exatamente uma LINHA.
- **Atributos**: Não possui


### 4.8 Relacionamento: PERCORRE (entre ROTA e TRECHO)
- **Cardinalidade**: M:N
  - Uma ROTA pode ser formada por vários TRECHOS.  
  - Um TRECHO pode ser compartilhado entre rotas distintas.  
- **Atributos**: Pode haver atributo de “ordem” se cada trecho tiver posição na rota <!-- discutir -->  


### 4.9 Relacionamento: DEFINE (entre TRECHO e PONTO)
- **Cardinalidade**: 2:N  <!-- discutir 2:N ou M:2 -->  
  - Um TRECHO corresponde a unicamente dois pontos.  
  - Um PONTO pode fazer parte de vários TRECHOS.
- **Atributos**: Não possui


### 4.10 Relacionamento: PERTENCE A (entre PARTIDA_PREVISTA e ROTA)
- **Cardinalidade**: N:1  <!-- discutir -->  
  - Diversas PARTIDAS_PREVISTAS podem pertencer a uma mesma ROTA (ou linha/itinerário específico).  
  - Cada PARTIDA_PREVISTA refere-se a apenas uma ROTA.
- **Atributos**: Não possui


### 4.11 Relacionamento: CUMPRE (entre VIAGEM e PARTIDA_PREVISTA)
- **Cardinalidade**: N:1  
  - Várias VIAGENS (em diferentes datas) podem cumprir a mesma PARTIDA_PREVISTA (horário fixo).  
  - Cada VIAGEM cumpre (corresponde a) uma única PARTIDA_PREVISTA.
- **Atributos**: Não possui


### 4.12 Relacionamento: FAZ (entre MOTORISTA e VIAGEM)
- **Cardinalidade**: 1:N  
  - Um MOTORISTA pode fazer várias VIAGENS (em momentos ou dias distintos).  
  - Cada VIAGEM é feita por exatamente um MOTORISTA.
- **Atributos**: Não possui


### 4.13 Relacionamento: SOFRE (entre VIAGEM e OCORRÊNCIA)
- **Cardinalidade**: 1:N  
  - Uma VIAGEM pode ter várias OCORRÊNCIAS (incidentes, eventos).  
  - Cada OCORRÊNCIA está associada a uma única VIAGEM.
- **Atributos**: Não possui


### 4.14 Relacionamento: TEM (entre USUÁRIO e BILHETE) <!-- discutir Sobre cobrador (pq motorista ou tecnico tbm n poderiam ter bilhete?) -->  
- **Cardinalidade**: 1:N  
  - Um USUÁRIO pode ter vários BILHETES (cartões).  <!-- discutir -->  
  - Cada BILHETE pertence a um único USUÁRIO.
- **Atributos**: Não possui


### 4.15 Relacionamento: É USADO EM (entre BILHETE e ENTRADA)
- **Cardinalidade**: 1:N  
  - Um BILHETE pode ser utilizado muitas vezes, gerando várias ENTRADAS (passagens).  
  - Cada ENTRADA (momento de passagem) está vinculada a exatamente um BILHETE.
- **Atributos**: Não possui

## 5. Especializações e Generalizações

### 5.1 Generalização: FUNCIONÁRIO (Superclasse)
- **Especializações**:  
  - **TÉCNICO** (Disjoint): Responsável por realizar manutenções  
    - **Atributos adicionais**: Não possui (herda os de FUNCIONÁRIO)  
  - **OPERADOR** (Disjoint): Atua na operação de veículos e cobranças  
    - **Atributos adicionais**: Não possui (herda os de FUNCIONÁRIO)


### 5.2 Generalização: OPERADOR (Superclasse)
- **Especializações**:  
  - **COBRADOR** (Overlap, se puder ser também motorista, ou Disjoint, se não puder): Atua na cobrança de passagens  
    - **Atributos adicionais**: Não possui (herda os de OPERADOR)  
  - **MOTORISTA** (Overlap ou Disjoint, dependendo do modelo): Conduz veículos de transporte  
    - **Atributos adicionais**:  
      - **cod_habilitacao**: Código/registro da habilitação (CNH)

## 6. Atributos Multivalorados e Compostos

### 6.1 Atributos Multivalorados
- **LINHA.tipos_pagamento**: Formas de pagamento aceitas pela linha.  
  - Exemplo de valores possíveis: {“Dinheiro”, “Cartão de Débito”, “Cartão de Crédito”, “Bilhete Eletrônico”}.  
  - Como atributo multivalorado, pode armazenar um conjunto de valores (1 ou mais) simultaneamente.

### 6.2 Atributos Compostos
- **GARAGEM.endereco**:  
  - Agrupa informações como *logradouro*, *numero*, *cidade* e *cep* em um atributo único.  
  - Internamente pode ser representado em partes (campos separados) ou como texto completo.  
  - Exemplo de componentes:  
    - **logradouro**: Rua, Avenida etc.  
    - **numero**: Número do endereço.  
    - **cidade**: Nome do município.  
    - **cep**: Código de Endereçamento Postal.

- **MANUTENCAO.localizacao** (possível atributo composto):  
  - Pode agrupar *latitude* e *longitude* em um único atributo denominado “localizacao”.  
  - Exemplo de componentes:  
    - **latitude**: Valor decimal da latitude.  
    - **longitude**: Valor decimal da longitude.


## 7. Restrições Adicionais (não representáveis no diagrama EER)

1. Uma estação terminal deve ter pelo menos 2 linhas associadas.
2. Um veículo só pode ser alocado a uma viagem se seu estado for "ativo".
3. O tempo médio de percurso de uma linha deve ser coerente com a soma dos tempos médios de seus trechos.
4. Bilhetes de integração têm um limite máximo de tempo entre a primeira e a última utilização.
5. Operadores não podem ser escalados para viagens consecutivas sem um intervalo mínimo de descanso.
6. Manutenções preventivas devem ser programadas com base na quilometragem ou no tempo desde a última manutenção.
7. As coordenadas geográficas de estações devem estar dentro dos limites da área de cobertura do sistema.
8. O número total de veículos ativos deve ser suficiente para atender à frequência definida para todas as linhas ativas.
9. Projetos de expansão só podem mudar para o status "em execução" após aprovação formal registrada.
10. Rotas de ida e volta de uma mesma linha devem ter correspondência em relação às estações atendidas.

## 8. Conclusão

O modelo EER apresentado captura os requisitos de dados identificados na etapa anterior, representando as entidades, relacionamentos, especializações e restrições do domínio de transporte público. Este modelo servirá como base para o projeto lógico utilizando o modelo Relacional na próxima etapa.

O diagrama completo está disponível na imagem `DiagramaEER.png` nesta pasta.
