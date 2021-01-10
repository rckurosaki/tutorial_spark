# 2. Conceitos

A arquitetura do Apache Spark™ é composta por duas principais abstrações:
* RDD (*Resilient Distributed Dataset*)
* DAG (*Directed Acyclic Graph*)

## 2.1. Resilient Distributed Dataset (RDD)
* *Resilient*: Tolerância a falhas, sendo capaz de recomputar partições danificadas provenientes de mau funcionamento dos nós;
* *Distributed*: Distribuído em multiplos nós de um cluster;
* *Dataset*: Representa as coleções de dados que serão processados. 

RDD é um dos principais tópicos para se entender como o Spark funciona. 
Resumidamente falando, são coleções de dados para apenas leitura que são particionados e distribuídos para diferentes nós de um *cluster* para poderem ser processados paralelamente. Essas coleções de dados são imutáveis e, para cada operação que envolva alguma mudança nos dados, uma nova RDD deve ser criada.
Há duas maneiras de se criar RDDs:
A primeira maneira tem como fonte um de um banco de dados, arquivos do sistema local, hdfs, entre outros.
A segunda maneira é copiar elementos de coleções para formar um *dataset* distribuído que possa ser processado em paralelo.

As principais características do RDD são:
* **Computação em memória**: Os dados são computados em memórias RAM distribuídas, o que aumenta significativamente a performance computacional. 

* **Particionamento**: É a peça principal para que o processamento possa ser paralelizado. Cada partição é uma divisão lógica de dados mutáveis.

* **Persistência**: Pode-se escolher qual RDD será reutilizada e armazená-la em memória ou em disco.

#### Operações
RDD suporta dois tipos de operações: **Transformações** e **Ações**.

Transformações são funções que recebem um RDD como *input* e produz uma ou mais RDDs como *output*. Dessa maneira, as transformações criam novos *datasets* a partir de um *dataset* existente.

Ações retornam o resultado final de uma computação RDD. Esse resultado é retornado para o *driver* do programa ou então escrito em um arquivo ou banco de dados externo. Ações são operações RDD que produzem valores "não RDD", ou seja, ele "*materializa*" um valor em um programa Spark. [[1]](https://spark.apache.org/docs/latest/rdd-programming-guide.html)[[2]](https://www.educba.com/rdd-in-spark/)[[3]](https://obstkel.com/apache-spark-concepts)[[4]](https://towardsdatascience.com/spark-71d0bc25a9ba)


## 2.2. Directed Acyclic Graph (DAG)
O *driver* Spark identifica as tarefas que podem ser computadas paralelamente e, a partir disso, contrói uma lógica de operações DAG.
DAG é um conjunto de vértices e arestas, onde os vértices representam os RDDs e as arestas representam as operações que serão aplicadas nesses RDDs. Na Figura 1 mostra como todas as arestas partem de uma operação anterior para uma próxima na sequência. 
Ela é um grafo finito direcionado sem ciclos. Esse modelo é uma generalização do modelo MapReduce, porém com otimizações.

![Esquema DAG](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.3QbSKq1YI0rTGlVGnM3WGgHaD4%26pid%3DApi&f=1)  
*** Figura 1: Esquema DAG ***

## 2.3. Modo de operação
Podemos usar o Spark localmente ou no modo cluster.

**Local**
No modo local, como o próprio nome diz, rodaremos o Spark no nosso computador ou em uma instância a nuvem. Esse modo é ideal para aprender a utilizar tudo o que ela tem para oferecer. A [prática](./prática.md) desse tutorial foi construída para ser executada em modo local.

**Cluster**
Como mostrado na Figura 2, no modo cluster o Spark funciona de maneira similar ao Hadoop com a arquitetura Coordenador-Subordinado. O Coordenador é chamado de *driver* e o Subordinado de *Executor*.
O *driver* mantém os metadados e as outras informações da aplicação, cuidando da distribuição dos dados nos diferentes nós e monitorando o trabalho dos processos subordinados.
Os nós subordinados executam os códigos e os reporta de volta para seu coordenador.[[1]](https://spark.apache.org/docs/latest/rdd-programming-guide.html)[[2]](https://www.educba.com/rdd-in-spark/)[[3]](https://obstkel.com/apache-spark-concepts)[[4]](https://towardsdatascience.com/spark-71d0bc25a9ba)

![Arquitetura Spark](/img/spark_arch.png)  
*** Figura 2: Arquitetura do Apache Spark™ ***