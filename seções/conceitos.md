# Conceitos

A arquitetura do Apache Spark é composta por duas principais abstrações:
* RDD (Resilient Distributed Dataset)
* DAG (Directed Acyclic Graph)

### Resilient Distributed Dataset - RDD
RDD é um dos principais tópicos para se entender como o Spark funciona. 
Resumidamente falando, são coleções de dados para apenas leitura que são particionados e distribuídos para diferentes nós de um cluster para poderem ser processados paralelamente. Essas coleções de dados são imutáveis e, para cada operação que envolva alguma mudança nos dados, uma nova RDD deve ser criada.
RDDs são estruturas com tolerância a falhas.
Há duas maneiras de se criar RDDs:
A primeira maneira tem como fonte um de um banco de dados, arquivos do sistema local, hdfs, entre outros.
A segunda maneira é copiar elementos de coleções para formar um dataset distribuído que possa ser processado em paralelo.


### Directed Acyclic Graph (DAG)

## Modo de operação
Podemos usar o Spark localmente ou no modo cluster.

**Local**
No modo local, como o próprio nome diz, rodaremos o Spark no nosso computador ou em uma instância a nuvem. Esse modo é ideal para aprender a utilizar tudo o que ela tem para oferecer. A [prática](./prática.md) desse tutorial foi construída para ser executada em modo local.

**Cluster**
No modo cluster, o Spark funciona de maneira similar ao Hadoop com a arquitetura Coordenador-Subordinado. O Coordenador é chamado de *driver* e o Subordinado de *Executor*.
O driver mantém os metadados e as outras informações da aplicação, cuidando da distribuição dos dados nos diferentes nós e monitorando o trabalho dos processos subordinados.
Os nós subordinados executam os códigos e os reporta de volta para seu coordenador.

[Arquitetura Spark](/img/spark_arch.png)
