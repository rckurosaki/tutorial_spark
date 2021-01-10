# 3. Quando usar e quando não usar o Spark?

## 3.1. Quando usar
A principal motivação de se usar o Spark é quanto temos que processar uma quantidade massiva de dados. Isso se aplica, por exemplo em aplicações de processamento de dados em tempo real , como dados colhidos por sensores, internet das coisas, setor financeiro, entre outros. Pode ser usado para tarefas de aprendizagem de máquina e também na construção de uma ETL.[[5]](https://www.toptal.com/spark/introduction-to-apache-spark)[[6]](https://blog.knoldus.com/do-you-really-need-spark-think-again/)[[7]](https://towardsdatascience.com/the-what-why-and-when-of-apache-spark-6c27abc19527)

## 3.2. Quem usa?

**Cientista de dados**

São responsáveis por tirar insights através da análise de dados. Para que isso possa ser feito, o Spark ajuda os cientistas de dados dando suporte durante grande parte do processo de análise como o acesso aos dados assim como a integração com a aprendizagem de máquina e visualização de dados.[[5]](https://www.toptal.com/spark/introduction-to-apache-spark)

**Engenheiro de dados**

São responsáveis por coletar, tratar e armazenar os dados. Spark ajuda os engenheiros de dados abstraindo a complexidade ao acesso de dados. Ele oferece *frameworks* fazendo com que o acesso aos dados, seja ele de um arquivo csv, json ou mesmo de um banco de dados relacional, seja muito simples.[[5]](https://www.toptal.com/spark/introduction-to-apache-spark)
A empresa de streaming de video e séries Netflix utiliza o Spark em sua solução de recomendações para os usuários.[[9]](https://netflixtechblog.com/netflix-at-spark-ai-summit-2018-5304749ed7fa)

**Indústria de jogos**

O processamento de dados para descobrir padrões e também para o processamento de dados em tempo real de eventos dos jogos *online*. São características muito importantes para a indústria, pois dessa maneira pode oferecer uma experiência de jogo mais prazerosa para o jogador como também melhorar anúncios personalizados, por exemplo.[[5]](https://www.toptal.com/spark/introduction-to-apache-spark)
A empresa de jogos online Riot Games, produtora de um dos jogos mais jogados no mundo League of Legends, utiliza o Spark para melhorar a experiencia dos jogadores em relação a problemas de rede e desbalanceamento do jogo coletando dados de partidas.[[10]](https://pt.slideshare.net/SparkSummit/video-games-at-scale-improving-the-gaming-experience-with-apache-spark)


Esses são apenas alguns exemplos de quem pode se beneficiar muito com o uso do Spark. 

## 3.3. Quando não usar

Analisar dados fazendo buscas específicas de algum dado não é o melhor caso de uso. Isso porque o Spark terá de carregar os dados em memória e depois fazer a busca pelo dado específico a cada busca desejada, ou seja, a performance não será boa. Neste caso, é recomendado o uso de um banco de dados! 

Inserir dados com frequência em uma coleção também não é uma boa ideia. Isso se deve ao fato de o Spark criar um novo arquivo a cada inserção de dados nessa coleção. 

Busca por conteúdo também não é um contexto onde o Spark é ideal. Um exemplo de busca por conteúdo seriam aquelas buscas que, conforme o usuário digita, a aplicação autocompleta a busca ou sugere outras buscas. Spark não deve ser usado nesse contexto, pois, para cada *request* feito, o Spark criaria uma *job*, e para cada *job*, ele iria consultar o banco de dados e carregar todo o conteúdo dessa busca. 
Para esse caso, há ferramentas muito mais eficientes como ElasticSearch, por exemplo.[[6]](https://blog.knoldus.com/do-you-really-need-spark-think-again/)[[7]](https://towardsdatascience.com/the-what-why-and-when-of-apache-spark-6c27abc19527)[[8]](https://www.pepperdata.com/blog/five-mistakes-to-avoid-when-using-spark/)


[Voltar para Sumário](/tutorial_spark#sumário)  
[Próxima Seção - Prática](/seções/prática.md)