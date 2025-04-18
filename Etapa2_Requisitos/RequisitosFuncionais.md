# Etapa 2 - Definição dos Requisitos Funcionais e de Dados da Aplicação

## 1. Introdução

O projeto prevê a implementação de um sistema com dados de uma rede de transporte público de ônibus para uso operacional e informativo. O sistema oferece funcionalidades para diferentes tipos de usuários, desde cidadãos até administradores ou aplicações de terceiros, permitindo consultas detalhadas sobre a rede existente e seus componentes. Na seção 2, apresentamos o funcionamento da rede e detalhamos seus perfis de uso. Na seção 3, apontamos requisitos mais detalhados de cada tipo de uso. Por fim, a seção 4, apontamos possíveis consultas e modificações frequentes a serem realizadas no banco de dados. Com base nestas especificações, procederemos ao desenvolvimento do projeto conceitual utilizando o modelo EER (Entidade-Relacionamento Estendido) na próxima etapa.

## 2. Perfis de uso

O sistema deve funcionar de forma genérica para qualquer rede de transporte coletivo por ônibus com cobrança de tarifa dentro dos veículos. Essa rede é operada por uma ou mais empresas de transporte que contratam os funcionários responsáveis pela operação (motorista e cobrador), administração e manutenção (técnico). Os funcionários cumprem escalas pré-determinadas por dia da semana. A operação dessa rede utiliza veículos de diferentes modelos que são estacionados em garagens fora dos horários operacionais. É nas garagens que os veículos são reabastecidos ou recarregados. Os modelos de veículo correspondem a uma determinada capacidade e se são movidos a diesel ou eletricidade. Um mesmo modelo pode ter mais de uma fabricante.

Cada veículo pode ser usado em várias viagens, que correspondem ao cumprimento de rotas de linhas diversas nos seus horários programados. Uma linha pode ter uma rota (circular) ou duas rotas, de ida e volta. Os veículos param em pontos pré-determinados no programa de viagem das rotas, percorrendo sempre um mesmo trecho entre dois pontos. Os trechos definem o intervalo de tempo e o desenho do trajeto entre os pontos. Conforme realiza as viagens, o sistema embarcado do ônibus atualiza sua localização geográfica. Essa localização e outros dados do sistema são públicos e podem ser consumidos por aplicações de terceiros. Uma viagem tem sempre um motorista, e pode ter um cobrador caso o motorista não acumule as duas funções.

Caso haja alguma ocorrência durante uma viagem, ela é registrada no sistema. Nem sempre uma viagem será iniciada exatamente no horário previsto. Veículos danificados ou que atingem determinada quilometragem rodada devem passar por manutenção, que é executada por um técnico.

A entrada na rede de transportes é feita com uso de um bilhete de transportes, que pode ser recarregado. Um usuário do sistema pode ter mais de um bilhete. A tarifa a ser paga pode depender da rota, do horário, do tipo de bilhete (estudante, comum, etc) e do veículo. Ainda, caso o passageiro faça o pagamento em dinheiro, deve ser registrada uma entrada usando um bilhete especial, portado pelo cobrador do veículo. Ao realizar a cobrança, o sistema embarcado no ônibus deve checar o saldo do bilhete e registrar a sua subtração na componente do sistema responsável pela bilhetagem.

A partir dessa especificação, podemos identificar os seguintes perfis de usuário e sistemas integrados:

### 2.1 Tipos de Usuários

- **Cidadãos/Passageiros**: Consultam informações sobre linhas, horários, trajetos e podem reportar problemas.
- **Funcionários operacionais da Rede de Transporte**: Funcionários que gerenciam as operações diárias das linhas e veículos.
- **Gestores de Manutenção**: Responsáveis pela manutenção de veículos, estações e infraestrutura.
- **Administradores do Sistema**: Gerenciam todos os aspectos do banco de dados e suas permissões.
- **Aplicações de terceiros**: Aplicativos desenvolvidos para que os usuários possam consultar detalhes sobre rotas e localização dos ônibus.

### 2.2 Sistemas integrados

- **Sistema de Embarcados (Ônibus)**: Envia dados sobre localização em tempo real, pagamento de passagens com cartão de transporte e ocorrências. Consome dados sobre ocorrências, saldo de cartões de transporte e programação das viagens.
- **Sistema de Bilhetagem Eletrônica**: Faz gestão do cadastro de usuários e do saldo dos cartões de transporte.

## 3. Requisitos

### 3.1 Consulta para Cidadãos

- 1: Consultar linhas de transporte por origem e destino
- 2: Visualizar horários de partida e chegada por linha e estação
- 3: Reportar problemas em veículos, pontos ou serviços
- 4: Receber alertas sobre mudanças em horários ou rotas

### 3.2 Gestão Operacional

- 5: Cadastrar e gerenciar ônibus
- 6: Cadastrar e gerenciar operadores e administradores
- 7: Programar escalas de serviços para operadores
- 8: Monitorar localização e status dos veículos em tempo real
- 9: Registrar e analisar ocorrências operacionais (atrasos, desvios, etc.)
- 10: Gerenciar horários e frequência de cada linha
- 11: Visualizar estatísticas operacionais (pontualidade, tempo médio de viagem)

### 3.3 Manutenção

- 12: Programar manutenções preventivas para veículos
- 13: Registrar manutenções corretivas realizadas
- 14: Gerenciar vida útil de componentes da frota
- 15: Analisar histórico de falhas por veículo/linha

### 3.4 Administração da Rede

- 16: Gerenciar usuários e permissões do sistema
- 17: Realizar backup e restauração de dados
- 18: Adicionar e remover serviços de transporte

## 4. Operações Frequentes

### 4.1 Consulta

- 1: Buscar linhas que conectam uma origem a um destino
- 2: Listar todos os horários de uma linha específica
- 3: Consultar veículos atualmente em operação
- 4: Verificar estatísticas de pontualidade por linha
- 5: Consultar histórico de ocorrências por tipo
- 6: Analisar demanda por horário e linha
- 7: Consultar rotas alternativas entre dois pontos
- 8: Visualizar histórico de manutenções por veículo
- 9: Consultar operadores disponíveis para uma determinada data
- 10: Analisar tendências de demanda por linha

### 4.2 Modificação

- 1: Registrar nova viagem
- 2: Atualizar status de um veículo
- 3: Adicionar nova linha ao sistema
- 4: Registrar manutenção realizada
- 5: Atualizar horários de uma linha
- 6: Registrar ocorrência operacional
- 7: Adicionar novo veículo à frota
- 8: Modificar rota de uma linha existente
- 9: Adicionar ou remover ponto
- 10: Atualizar escala de operadores
- 11: Adicionar novo operador ao sistema
