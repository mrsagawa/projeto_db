# Etapa 2 - Definição dos Requisitos Funcionais e de Dados da Aplicação

## 1. Descrição Geral da Aplicação

O Sistema de Gestão e Planejamento de Transporte Público (SGPTP) é uma aplicação de banco de dados projetada para auxiliar na administração, manutenção e expansão estratégica da rede de transporte público urbano. O sistema oferece funcionalidades para diferentes tipos de usuários, desde cidadãos comuns até gestores de transporte e planejadores urbanos, permitindo consultas detalhadas sobre a rede existente, análise de demanda e simulações para expansão.

## 2. Usuários e Sistemas Externos

### 2.1 Tipos de Usuários

- **Cidadãos/Passageiros**: Consultam informações sobre linhas, horários, trajetos e podem reportar problemas.
- **Operadores de Transporte**: Funcionários que gerenciam as operações diárias das linhas e veículos.
- **Gestores de Manutenção**: Responsáveis pela manutenção de veículos, estações e infraestrutura.
- **Administradores do Sistema**: Gerenciam todos os aspectos do banco de dados e suas permissões.

### 2.2 Sistemas Externos

- **Sistema de Embarcados (Onibus)**: 
- **Sistema de Bilhetagem Eletrônica**: Fornece dados sobre o fluxo de passageiros.
- **Sistema de GPS dos Veículos**: Fornece dados de localização em tempo real.

## 3. Requisitos Funcionais

### 3.1 Módulo de Consulta para Cidadãos

- RF01: Consultar linhas de transporte por origem e destino
- RF02: Visualizar horários de partida e chegada por linha e estação
- RF03: Calcular rotas otimizadas entre dois pontos (menor tempo, menor distância, menor custo)
- RF04: Visualizar mapa da rede com localização das estações e pontos
- RF05: Consultar histórico de demanda por linha e horário
- RF06: Reportar problemas em veículos, estações ou serviços
- RF07: Receber alertas sobre mudanças em horários ou rotas

### 3.2 Módulo de Gestão Operacional

- RF08: Cadastrar e gerenciar ônibus
- RF09: Cadastrar e gerenciar operadores e motoristas
- RF10: Programar escalas de serviços para veículos e operadores
- RF11: Monitorar localização e status dos veículos em tempo real
- RF12: Registrar e analisar ocorrências operacionais (atrasos, desvios, etc.)
- RF13: Gerenciar horários e frequência de cada linha
- RF14: Visualizar estatísticas operacionais (pontualidade, tempo médio de viagem)

### 3.3 Módulo de Manutenção

- RF15: Programar manutenções preventivas para veículos
- RF16: Registrar manutenções corretivas realizadas
- RF17: Gerenciar vida útil de componentes da frota
- RF18: Controlar estoque de peças e insumos
- RF19: Programar manutenções de estações e vias
- RF20: Gerar relatórios de eficiência e custos de manutenção
- RF21: Analisar histórico de falhas por veículo/linha

### 3.4 Módulo Administrativo

- RF22: Gerenciar usuários e permissões do sistema
- RF23: Realizar backup e restauração de dados
- RF24: Configurar parâmetros globais do sistema
- RF25: Gerar relatórios gerenciais consolidados
- RF26: Add services and routes (change!)

## 4. Requisitos de Dados

### 4.1 Entidades Principais e seus Atributos

#### 4.1.1 Rede de Transporte

- **Linha**
  - Identificador único
  - Nome/Número da linha
  - Tipo (ônibus, metrô, trem, VLT, etc.)
  - Status (ativa, desativada, em manutenção)
  - Data de inauguração
  - Extensão total (km)
  - Tempo médio de percurso
  - Capacidade de passageiros/hora
  - Tarifa

- **Estação/Ponto**
  - Identificador único
  - Nome da estação/ponto
  - Tipo (terminal, estação, ponto de parada)
  - Status (operacional, em manutenção, desativada)
  - Localização geográfica (coordenadas)
  - Endereço
  - Acessibilidade (true/false)
  - Capacidade (passageiros)
  - Horário de funcionamento
  - Infraestrutura disponível

- **Trecho**
  - Identificador único
  - Estação de origem
  - Estação de destino
  - Distância (km)
  - Tempo médio de percurso
  - Estado de conservação
  - Data da última manutenção
  - Tipo de via

- **Rota**
  - Identificador único
  - Linha associada
  - Sequência de estações/pontos
  - Direção (ida/volta)
  - Horário de início de operação
  - Horário de fim de operação
  - Frequência (intervalo entre veículos)
  - Dias de operação

#### 4.1.2 Frota e Recursos

- **Veículo**
  - Identificador único
  - Tipo (ônibus, vagão, etc.)
  - Modelo
  - Placa/Registro
  - Capacidade (passageiros)
  - Ano de fabricação
  - Data de aquisição
  - Estado (ativo, em manutenção, desativado)
  - Quilometragem acumulada
  - Tipo de combustível/energia
  - Acessibilidade (true/false)
  - Equipamentos (ar condicionado, Wi-Fi, etc.)

- **Operador**
  - Identificador único
  - Nome completo
  - Cargo (motorista, cobrador, etc.)
  - Registro profissional
  - Data de admissão
  - Qualificações
  - Status (ativo, afastado, etc.)
  - Linha(s) de atuação
  - Horário de trabalho

- **Manutenção**
  - Identificador único
  - Tipo (preventiva, corretiva)
  - Data de início
  - Data de conclusão
  - Descrição do serviço
  - Custo
  - Responsável técnico
  - Peças substituídas
  - Veículo ou instalação associada
  - Status (agendada, em andamento, concluída)

#### 4.1.3 Operação e Demanda

- **Viagem**
  - Identificador único
  - Linha associada
  - Rota específica
  - Veículo alocado
  - Operador responsável
  - Horário programado de partida
  - Horário real de partida
  - Horário programado de chegada
  - Horário real de chegada
  - Status da viagem
  - Número de passageiros
  - Ocorrências registradas

- **Ocorrência**
  - Identificador único
  - Tipo (atraso, quebra, acidente, etc.)
  - Data e hora
  - Localização
  - Descrição
  - Gravidade
  - Solução adotada
  - Tempo de resolução
  - Impacto operacional

- **Demanda**
  - Identificador único
  - Linha/Rota associada
  - Data 
  - Faixa horária
  - Volume de passageiros
  - Tipo de dia (útil, sábado, domingo, feriado)
  - Taxa de ocupação
  - Origem das informações

- **Bilhete/Passagem**
  - Identificador único
  - Tipo (unitário, integração, mensal, etc.)
  - Data e hora de utilização
  - Valor
  - Estação/ponto de entrada
  - Estação/ponto de saída
  - Linha(s) utilizada(s)
  - ID do usuário (quando disponível)

#### 4.1.4 Planejamento e Expansão

- **Projeto de Expansão**
  - Identificador único
  - Nome do projeto
  - Descrição
  - Status (em análise, aprovado, em execução, concluído)
  - Data de início prevista
  - Data de conclusão prevista
  - Orçamento estimado
  - Região impactada
  - Benefícios esperados
  - Responsável

- **Simulação**
  - Identificador único
  - Projeto associado
  - Data de criação
  - Parâmetros utilizados
  - Resultados obtidos
  - Demanda projetada
  - Custo operacional projetado
  - Impacto na rede existente
  - Validação (aprovada/rejeitada)

## 5. Operações Frequentes

### 5.1 Operações de Consulta

- Q01: Buscar linhas que conectam uma origem a um destino
- Q02: Listar todos os horários de uma linha específica
- Q03: Verificar tempo médio de espera em cada estação
- Q04: Consultar veículos atualmente em operação
- Q05: Listar manutenções programadas para a próxima semana
- Q06: Verificar estatísticas de pontualidade por linha
- Q07: Consultar histórico de ocorrências por tipo
- Q08: Analisar demanda por horário e linha
- Q09: Listar estações por volume de passageiros
- Q10: Verificar índice de ocupação por linha e horário
- Q11: Consultar rotas alternativas entre dois pontos
- Q12: Visualizar histórico de manutenções por veículo
- Q13: Listar projetos de expansão por status
- Q14: Consultar operadores disponíveis para uma determinada data
- Q15: Analisar tendências de demanda por região

### 5.2 Operações de Modificação

- M01: Registrar nova viagem
- M02: Atualizar status de um veículo
- M03: Adicionar nova linha ao sistema
- M04: Registrar manutenção realizada
- M05: Atualizar horários de uma linha
- M06: Registrar ocorrência operacional
- M07: Adicionar novo veículo à frota
- M08: Atualizar status de um projeto de expansão
- M09: Registrar embarque/desembarque de passageiros
- M10: Atualizar dados de demanda diária
- M11: Modificar rota de uma linha existente
- M12: Adicionar ou remover estação/ponto
- M13: Atualizar escala de operadores
- M14: Registrar resultados de simulação
- M15: Adicionar novo operador ao sistema

## 6. Restrições e Regras de Negócio

- RN01: Uma linha deve ter pelo menos duas estações/pontos
- RN02: Um veículo só pode estar alocado a uma viagem por vez
- RN03: Manutenções preventivas devem ser programadas com base na quilometragem ou tempo de uso
- RN04: O intervalo entre veículos de uma mesma linha não pode ser inferior ao tempo mínimo definido
- RN05: Todo veículo deve passar por inspeção após determinada quilometragem
- RN06: O tempo de descanso entre jornadas dos operadores deve respeitar a legislação trabalhista
- RN07: Projetos de expansão só podem ser executados após aprovação completa
- RN08: Alterações em rotas ou horários devem ser comunicadas com antecedência mínima
- RN09: Bilhetes com integração têm tempo máximo para utilização
- RN10: Relatórios de demanda devem ser gerados diariamente
- RN11: Alocação de veículos deve considerar capacidade e demanda esperada
- RN12: Veículos com acessibilidade devem ser priorizados em horários de maior demanda
- RN13: O sistema deve manter histórico de alterações por pelo menos 5 anos
- RN14: Simulações devem considerar dados históricos de pelo menos 1 ano
- RN15: Estações terminais devem ter infraestrutura mínima definida

Este documento define os requisitos fundamentais para o desenvolvimento do Sistema de Gestão e Planejamento de Transporte Público. Com base nestas especificações, procederemos ao desenvolvimento do projeto conceitual utilizando o modelo EER (Entidade-Relacionamento Estendido) na próxima etapa.
