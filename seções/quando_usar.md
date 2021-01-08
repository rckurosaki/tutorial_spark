## Quando usar e quando não usar o Spark?
### Quando usar
A principal motivação de se usar o Spark é quanto temos que processar uma quantidade massiva de dados. Isso se aplica, por exemplo em aplicações de processamento de dados em tempo real , como dados colhidos por sensores, internet das coisas, setor financeiro, entre outros. Pode ser usado para tarefas de aprendizagem de máquina e também na construção de uma ETL.

#### Quem usa?
**Cientista de dados**
![Ciência de dados](/img/data_science.jpg)
São responsáveis por tirar insights através da análise de dados. Para que isso possa ser feito, o Spark ajuda os cientistas de dados dando suporte durante grande parte do processo de análise como o acesso aos dados assim como a integração com a aprendizagem de máquina e visualização de dados.

**Engenheiro de dados**
![Engenheiro de dados](/img/data_engineering.jpg)
São responsáveis por coletar, tratar e armazenar os dados. Spark ajuda os engenheiros de dados abstraindo a complexidade ao acesso de dados. Ele oferece *frameworks* fazendo com que o acesso aos dados, seja ele de um arquivo csv, json ou mesmo de um banco de dados relacional, seja muito simples.

**Indústria de jogos**
![Indústria de jogos](/img/game_industry.jpg)
O processamento de dados para descobrir padrões e também para o processamento de dados em tempo real de eventos dos jogos *online*. São características muito importantes para a indústria, pois dessa maneira pode oferecer uma experiência de jogo mais prazerosa para o jogador como também melhorar anúncios personalizados, por exemplo.

Esses são apenas alguns exemplos de quem pode se beneficiar muito com o uso do Spark. 

### Quando não usar

Analisar dados fazendo buscas específicas de algum dado não é o melhor caso de uso. Isso porque o Spark terá de carregar os dados em memória e depois fazer a busca pelo dado específico a cada busca desejada, ou seja, a performance não será boa. Neste caso, é recomendado o uso de um banco de dados! 

Inserir dados com frequência em uma coleção também não é uma boa ideia. Isso se deve ao fato de o Spark criar um novo arquivo a cada inserção de dados nessa coleção. 

Busca por conteúdo também não é um contexto onde o Spark é ideal. Um exemplo de busca por conteúdo seriam aquelas buscas que, conforme o usuário digita, a aplicação autocompleta a busca ou sugere outras buscas. Spark não deve ser usado nesse contexto, pois, para cada *request* feito, o Spark criaria uma *job*, e para cada *job*, ele iria consultar o banco de dados e carregar todo o conteúdo dessa busca. 
Para esse caso, há ferramentas muito mais eficientes como ElasticSearch, por exemplo. 

