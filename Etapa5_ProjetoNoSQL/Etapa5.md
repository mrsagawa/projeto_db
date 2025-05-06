Para esta etapa do projeto, optamos por utilizar um banco de dados NoSQL baseado em documentos, como o MongoDB. Essa escolha foi feita com base na análise dos requisitos funcionais e operacionais definidos nas etapas anteriores e na notação abordada em aula, voltada à representação de dados semiestruturados (JSON/árvore).

O modelo de documentos é particularmente apropriado para aplicações com dados estruturados de forma hierárquica, como é o caso da nossa rede de transporte público. Muitas entidades do sistema — como usuários com seus bilhetes, veículos com seu histórico de manutenções, viagens com ocorrências e escalas de operadores — possuem uma estrutura naturalmente aninhada e autocontida, o que torna a modelagem por documentos mais direta e eficiente.

Além disso, a maior parte das consultas e operações frequentes do sistema são focadas em uma única entidade e seus dados relacionados, como por exemplo:

- Consultar os bilhetes de um usuário;
- Registrar uma ocorrência em uma viagem;
- Atualizar o status de um veículo;
- Verificar o histórico de manutenções de um ônibus.

Todas essas ações podem ser executadas com eficiência sobre documentos individuais, sem necessidade de operações complexas de junção, o que melhora o desempenho e simplifica o código da aplicação.

Outros modelos NoSQL, como bancos de grafos ou famílias de colunas, não se mostraram tão adequados. O modelo em grafos é mais indicado quando há relações altamente conectadas e navegação relacional complexa, o que não é o caso predominante nesta aplicação. Já o modelo colunar é mais eficiente para grandes volumes de dados tabulares com acesso massivo por chave, mas tem baixa flexibilidade para representar estruturas aninhadas ou registros com múltiplos subcomponentes, como bilhetes, escalas ou manutenções.

Por fim, o modelo de documentos também é totalmente compatível com a notação trabalhada nas aulas, que utiliza representações em JSON e estruturas em árvore, o que favorece tanto o aprendizado quanto a implementação da solução.

Dessa forma, o uso de um banco de documentos proporciona:

- Flexibilidade estrutural;
- Facilidade de leitura e escrita;
- Eficiência no armazenamento e acesso a dados aninhados;
- Aderência à modelagem orientada ao domínio do problema.

Por esses motivos, o modelo de documentos foi escolhido como a solução NoSQL mais adequada para esta aplicação.

