# Etapa 3 - Projeto Conceitual do Banco de Dados usando o Modelo EER

## 1. Introdução

Este documento apresenta o projeto conceitual do banco de dados para o Sistema de Gestão e Planejamento de Transporte Público (SGPTP) utilizando o Modelo Entidade-Relacionamento Estendido (EER). O modelo EER permite representar com maior precisão as abstrações e restrições semânticas identificadas durante a análise de requisitos.

## 2. Diagrama EER

TODO!

## 3. Entidades e Seus Atributos

### 3.1 Entidade: LINHA

#### Descrição
Representa as linhas de transporte público disponíveis na rede.

#### Atributos
- **id_linha** (Chave primária): Identificador único da linha
- **nome**: Nome ou número da linha
- **status**: Estado atual da linha (ativa, desativada, em manutenção)
- **data_inauguracao**: Data em que a linha começou a operar
- **extensao_total**: Comprimento total da linha em quilômetros
- **tarifa**: Valor da passagem

### 3.2 Entidade: PONTO

#### Descrição
Representa as estações, terminais e pontos de parada da rede de transporte.

#### Atributos
- **id_ponto** (Chave primária): Identificador único do ponto
- **nome**: Nome do ponto
- **tipo**: Classificação (terminal, estação, ponto de parada)
- **status**: Estado atual (operacional, em manutenção, desativada)
- **latitude**: Coordenada geográfica (latitude)
- **longitude**: Coordenada geográfica (longitude)
- **endereco**: Endereço completo
- **acessibilidade**: Indica se possui recursos de acessibilidade (booleano)

### Entidade: TERMINAL

#### Descrição
Representa as estações, terminais e pontos de parada da rede de transporte.

#### Atributos
- **id_terminal** (Chave primária): Identificador único de terminal
- **nome**: Nome do terminal
- **opening hours**:
- **closing hours**: 

### TERMINAL_PONTO:
#### Descrição

#### Atributos
- **id_terminal** (Chave entrangeira): Identificador único de terminal
- **ids_pontos**: (chave primaria)


<!-- #### Atributos
- **id_trecho** (Chave primária): Identificador único do trecho
- **distancia**: Distância em quilômetros
- **tempo_medio**: Tempo médio necessário para percorrer o trecho
- **estado_conservacao**: Condição atual do trecho
- **data_ultima_manutencao**: Data da última manutenção realizada
- **tipo_via**: Característica da via (exclusiva, compartilhada, etc.) -->

### 3.3 Entidade: ROTA

#### Descrição
Representa o conjunto ordenado de estações que forma um itinerário específico.

#### Atributos
- **id_rota** (Chave primária): Identificador único da rota
- **direcao**: Sentido da rota (ida/volta)
- **horario_inicio_operacao**: Horário da primeira viagem
- **horario_fim_operacao**: Horário da última viagem
- **frequencia**: Intervalo entre veículos (em minutos)
- **dias_operacao**: Dias da semana em que a rota opera

### 3.5 Entidade: ONIBUS

#### Descrição
Representa os veículos utilizados para o transporte de passageiros.

#### Atributos
- **id_veiculo** (Chave primária): Identificador único do veículo
- **modelo**: Modelo/fabricante
- **placa**: Placa ou registro oficial
- **capacidade**: Número máximo de passageiros
- **ano_fabricacao**: Ano em que foi fabricado
- **data_aquisicao**: Data em que foi adquirido pela empresa
- **estado**: Condição atual (ativo, em manutenção, desativado)
- **quilometragem**: Quilometragem acumulada
- **tipo_combustivel**: Tipo de energia utilizada
- **acessibilidade**: Indica se possui recursos de acessibilidade (booleano)
- **equipamentos**: Lista de equipamentos disponíveis

### 3.6 Entidade: OPERADOR

#### Descrição
Representa os funcionários que operam os veículos e serviços.

#### Atributos
- **id_operador** (Chave primária): Identificador único do operador
- **nome**: Nome completo
- **cargo**: Função exercida (motorista, cobrador, etc.)
- **registro**: Número de registro profissional
- **data_admissao**: Data de contratação
- **qualificacoes**: Certificações e habilitações
- **status**: Situação atual (ativo, afastado, etc.)

### 3.7 Entidade: MANUTENCAO

#### Descrição
Representa os serviços de manutenção realizados em veículos ou infraestrutura.

#### Atributos
- **id_manutencao** (Chave primária): Identificador único da manutenção
- **tipo**: Classificação (preventiva, corretiva)
- **data_inicio**: Data de início do serviço
- **data_conclusao**: Data de finalização do serviço
- **descricao**: Detalhamento do serviço realizado
- **custo**: Valor gasto
- **responsavel**: Técnico responsável
- **pecas_substituidas**: Lista de componentes substituídos
- **status**: Estado atual (agendada, em andamento, concluída)

### 3.8 Entidade: VIAGEM

#### Descrição
Representa cada viagem individual realizada por um veículo em uma rota.

#### Atributos
- **id_viagem** (Chave primária): Identificador único da viagem
- **horario_prog_partida**: Horário programado para início
- **horario_real_partida**: Horário real de início
- **horario_prog_chegada**: Horário programado para conclusão
- **horario_real_chegada**: Horário real de conclusão
- **status**: Estado da viagem (programada, em andamento, concluída, cancelada)
- **num_passageiros**: Quantidade de passageiros transportados

### 3.9 Entidade: OCORRENCIA

#### Descrição
Representa eventos não planejados que afetam a operação normal.

#### Atributos
- **id_ocorrencia** (Chave primária): Identificador único da ocorrência
- **tipo**: Categoria (atraso, quebra, acidente, etc.)
- **data_hora**: Momento em que aconteceu
- **localizacao**: Local do evento
- **descricao**: Detalhamento
- **gravidade**: Nível de severidade
- **solucao**: Medidas tomadas
- **tempo_resolucao**: Tempo necessário para resolver
- **impacto**: Efeito na operação

### 3.10 Entidade: DEMANDA

#### Descrição
Representa os dados de fluxo de passageiros.

#### Atributos
- **id_demanda** (Chave primária): Identificador único do registro de demanda
- **data**: Data da coleta
- **faixa_horaria**: Período específico
- **volume_passageiros**: Quantidade de passageiros
- **tipo_dia**: Classificação (útil, sábado, domingo, feriado)
- **taxa_ocupacao**: Percentual de ocupação
- **origem_dados**: Fonte das informações

### 3.11 Entidade: BILHETE

#### Descrição
Representa os bilhetes ou passagens utilizados pelos passageiros.

#### Atributos
- **id_bilhete** (Chave primária): Identificador único do bilhete
- **tipo**: Categoria (unitário, integração, mensal, etc.)
- **data_hora_utilizacao**: Momento em que foi utilizado
- **valor**: Preço pago
- **id_usuario**: Identificador do usuário (quando disponível)


## 4. Relacionamentos

### 4.1 Relacionamento: COMPOSTA_POR (entre LINHA e ROTA)
- **Cardinalidade**: 1:N (Uma linha pode ter múltiplas rotas, mas cada rota pertence a exatamente uma linha)
- **Atributos**: Não possui

### 4.2 Relacionamento: PASSA_POR (entre ROTA e ESTACAO)
- **Cardinalidade**: N:M (Uma rota passa por várias estações, e uma estação pode pertencer a várias rotas)
- **Atributos**:
  - **ordem**: Posição da estação na sequência da rota
  - **tempo_parada**: Tempo médio de parada na estação

### 4.3 Relacionamento: CONECTA (entre TRECHO e ESTACAO)
- **Cardinalidade**: N:M (Um trecho conecta duas estações, e uma estação pode estar conectada a vários trechos)
- **Atributos**: 
  - **tipo_conexao**: Natureza da conexão (direta, baldeação)

### 4.4 Relacionamento: REALIZA (entre VEICULO e VIAGEM)
- **Cardinalidade**: 1:N (Um veículo pode realizar múltiplas viagens, mas cada viagem é realizada por exatamente um veículo)
- **Atributos**: Não possui

### 4.5 Relacionamento: EXECUTA (entre OPERADOR e VIAGEM)
- **Cardinalidade**: 1:N (Um operador pode executar múltiplas viagens, mas cada viagem é executada por exatamente um operador)
- **Atributos**: Não possui

### 4.6 Relacionamento: SEGUE (entre VIAGEM e ROTA)
- **Cardinalidade**: N:1 (Múltiplas viagens podem seguir a mesma rota, e cada viagem segue exatamente uma rota)
- **Atributos**: Não possui

### 4.7 Relacionamento: REGISTRA (entre VIAGEM e OCORRENCIA)
- **Cardinalidade**: 1:N (Uma viagem pode registrar múltiplas ocorrências, e cada ocorrência está associada a exatamente uma viagem)
- **Atributos**: Não possui

### 4.8 Relacionamento: RECEBE (entre VEICULO e MANUTENCAO)
- **Cardinalidade**: 1:N (Um veículo pode receber múltiplas manutenções, e cada manutenção é realizada em exatamente um veículo)
- **Atributos**: Não possui

### 4.9 Relacionamento: POSSUI (entre ESTACAO e MANUTENCAO)
- **Cardinalidade**: 1:N (Uma estação pode receber múltiplas manutenções, e cada manutenção é realizada em exatamente uma estação)
- **Atributos**: Não possui

### 4.10 Relacionamento: COLETA (entre LINHA e DEMANDA)
- **Cardinalidade**: 1:N (Uma linha pode ter múltiplos registros de demanda, e cada registro pertence a exatamente uma linha)
- **Atributos**: Não possui

### 4.11 Relacionamento: MONITORA (entre ESTACAO e DEMANDA)
- **Cardinalidade**: 1:N (Uma estação pode ter múltiplos registros de demanda, e cada registro pertence a exatamente uma estação)
- **Atributos**: Não possui

### 4.12 Relacionamento: UTILIZA (entre BILHETE e VIAGEM)
- **Cardinalidade**: N:M (Um bilhete pode ser utilizado em várias viagens [integração], e uma viagem pode ter múltiplos bilhetes)
- **Atributos**: 
  - **timestamp_utilizacao**: Momento exato da utilização
  - **estacao_entrada**: Estação onde o bilhete foi validado
  - **estacao_saida**: Estação onde o passageiro saiu (quando aplicável)

### 4.13 Relacionamento: PROPOE (entre PROJETO_EXPANSAO e LINHA)
- **Cardinalidade**: 1:N (Um projeto pode propor múltiplas linhas novas, e cada proposta de linha está associada a exatamente um projeto)
- **Atributos**: 
  - **impacto_estimado**: Efeito previsto na rede existente
  - **prioridade**: Nível de importância da linha no projeto

### 4.14 Relacionamento: ANALISA (entre SIMULACAO e PROJETO_EXPANSAO)
- **Cardinalidade**: N:1 (Múltiplas simulações podem analisar o mesmo projeto, e cada simulação está associada a exatamente um projeto)
- **Atributos**: 
  - **cenario**: Descrição do cenário simulado
  - **confiabilidade**: Nível de confiança dos resultados

## 5. Especializações e Generalizações

### 5.1 Generalização: ESTACAO (Superclasse)
- **Especializações**: 
  - **TERMINAL** (Disjoint): Estação maior com múltiplas linhas e estrutura completa
    - Atributos adicionais: 
      - **num_plataformas**: Quantidade de plataformas
      - **area_total**: Área em metros quadrados
      - **servicos_disponiveis**: Lista de serviços oferecidos
  - **PONTO_INTERMEDIARIO** (Disjoint): Parada comum ao longo da linha
    - Atributos adicionais:
      - **tipo_abrigo**: Tipo de estrutura (coberto, parcial, sem abrigo)
      - **sinalizacao**: Tipo de sinalização presente

### 5.2 Generalização: VEICULO (Superclasse)
- **Especializações**:
  - **ONIBUS** (Disjoint): Veículo rodoviário
    - Atributos adicionais:
      - **comprimento**: Tamanho do veículo
      - **num_portas**: Quantidade de portas
      - **articulado**: Indica se é articulado (booleano)
  - **VAGAO** (Disjoint): Veículo ferroviário
    - Atributos adicionais:
      - **potencia**: Capacidade do motor
      - **sistema_tracao**: Tipo de tecnologia
      - **capacidade_pe**: Número de passageiros em pé
      - **capacidade_sentado**: Número de passageiros sentados

### 5.3 Generalização: OPERADOR (Superclasse)
- **Especializações**:
  - **MOTORISTA** (Disjoint): Conduz veículos rodoviários
    - Atributos adicionais:
      - **num_cnh**: Número da Carteira Nacional de Habilitação
      - **categoria_cnh**: Categoria da habilitação
      - **validade_cnh**: Data de vencimento da habilitação
  - **CONDUTOR_TREM** (Disjoint): Opera veículos ferroviários
    - Atributos adicionais:
      - **certificacao**: Tipo de certificação técnica
      - **horas_treinamento**: Quantidade de horas de treinamento

## 6. Atributos Multivalorados e Compostos

### 6.1 Atributos Multivalorados
- **LINHA.tipos_pagamento**: Formas de pagamento aceitas
- **ESTACAO.servicos**: Serviços disponíveis na estação
- **VEICULO.equipamentos**: Lista de equipamentos presentes
- **OPERADOR.qualificacoes**: Certificações e habilitações

### 6.2 Atributos Compostos
- **ESTACAO.localizacao**: Composto por latitude e longitude
- **OPERADOR.contato**: Composto por telefone, email e endereço
- **PROJETO_EXPANSAO.cronograma**: Composto por etapas, cada uma com data inicial e final
- **SIMULACAO.resultados**: Composto por diferentes métricas e suas respectivas avaliações

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
