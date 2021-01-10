# Tutorial Apache Spark™

Projeto final da disciplina Processamento Massivo de Dados do curso de Ciências da Computação da Universidade Federal de São Carlos campus Sorocaba.
Ministrada pela Profª Drª Sahudy Montenegro González. 

### Integrantes do grupo autores deste documento:
- Lucas Sampaio de Souza - 743568
- Renato Araujo Rizzo - 587788
- Renato Candido Kurosaki - 587834

# Introdução

O objetivo da disciplina de Processamento Massivo de Dados em relação a este projeto final foi capacitar os alunos a entenderem conceitos de *big data*, conhecerem as características e dificuldades de se manipular volumes massivos de dados e descobrir como o mercado, a indústria e até mesmo o usuário comum utilizam ferramentas para colocar em prática estes conceitos e superar essas dificuldades. 
Esta documentação e tutorial é o resultado de pesquisas e estudos realizados pelos integrantes do grupo sobre a ferramenta Apache Spark™, tecnologia essa que une características de processamento distribuído, paralelo e tolerante a falhas para possibilitar manipulação eficiente de grandes volumes de dados.

A intenção do projeto também é disponibilizar esse conhecimento a qualquer um que tenha interesse na ferramenta estudada e queira ter uma experiência prática introdutória com a mesma.

A documentação está organizada por seções iniciando por uma explicação do que é o Apache Spark™, os principais conceitos da ferramenta, alguns casos de uso e por fim um tutorial prático com Spark em um caso de uso específico. Um apêndice foi incluído para complementar o tutorial assim como outras seções por requisito de entrega do projeto em si.

# Sumário

* [1. Sobre](/seções/sobre.md)
* [2. Conceitos](/seções/conceitos.md)
    * [2.1. Resilient Distributed Dataset (RDD)](/seções/conceitos.md#21-resilient-distributed-dataset-(RDD))
    * [2.2. Directed Acyclic Graph (DAG)](/seções/conceitos.md#22-directed-acyclic-graph-(DAG))
    * [2.3. Modo de operação](/seções/conceitos.md#23-modo-de-operação)
* [3. Quando usar e quando não usar o Spark?](/seções/quando_usar.md)
    * [3.1. Quando usar](/seções/quando_usar.md#31-quando-usar)
    * [3.2. Quem usa?](/seções/quando_usar.md#32-quem-usa?)
    * [3.3. Quando não usar](/seções/quando_usar.md#33-quando-não-usar)
* [4. Prática](/seções/prática.md)
    * [4.1. Requisitos de configuração da máquina](/seções/prática.md#41-requisitos-de-configuração-da-máquina)
    * [4.2. Softwares utilizados](/seções/prática.md#42-softwares-utilizados)
    * [4.3. Instalação do pyspark](/seções/prática.md#43-instalação-do-pyspark)
    * [4.4. Executar](/seções/prática.md#44-executar)
    * [4.5. Projeto](/seções/prática.md#45-projeto)
    * [4.6. Implementação](/seções/prática.md#46-implementação)
* [5. Conclusão](/seções/conclusao.md)
* [6. Referências](/seções/referências.md)
* [7. Apêndice A](/seções/criando_sqlite.md)
    * [7.1. Montando um banco de dados relacional SQLite](/seções/criando_sqlite.md#71-montando-um-banco-de-dados-relacional-sqlite)
