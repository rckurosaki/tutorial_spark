# Prática

### Projeto
Para a parte prática do tutorial, iremos montar um projeto que implementa um ETL (**E**xtract, **T**ransform, **L**oad). Isto é, iremos extrair os dados de uma fonte, realizaremos o processamento desses dados e depois iremos armazená-los em outro lugar.
A ideia será extrair os dados que estão em um modelo relacional, fazer uma agregação para transformar em um modelo de documento e salvar em um arquivo JSON.
Esse projeto irá utilizar os dados da *[Northwind sample database](https://docs.yugabyte.com/latest/sample-data/northwind/)*. 
Essa base de dados contém dados de vendas de uma companhia fictícia chamada *Northwind Traders*. Essa empresa importa e exporta produtos alimentícios por todo o mundo.


### A base de dados
A base de dados completa contém 14 tabelas em um modelo relacional.  
![Diagrama base de dados](/img/diagrama_completo.png)
Para o nosso projeto, apenas utilizaremos 5 dessas tabelas, sendo elas:
* customers
* orders
* order_details 
* products
* categories

![Tabelas utilizadas no projeto](/img/diagrama_blur.png)
Nosso objetivo final será agregar todas as compras, produtos e detalhes dos produtos realizadas por um cliente, tendo como resultado final um documento com as seguintes características:

```
{
	CustomerID
	CompanyName
	Address
	City
	Region
	Country
	Orders{[
		OrderID
		OrderDetails{[
			ProductID
			Products{[
				CategoryID
				CategoryName
				Description
				ProductName
				UnitPrice
			]}
		]}
	]}
}
```

## Implementação

### Obter os arquivos necessários
Inicialmente, temos que fazer o download de todos os arquivos csv que iremos utilizar.
Para isso, utilizamos o software `wget`.
```
wget https://raw.githubusercontent.com/tmcnab/northwind-mongo/master/orders.csv
wget https://raw.githubusercontent.com/tmcnab/northwind-mongo/master/customers.csv
wget https://raw.githubusercontent.com/tmcnab/northwind-mongo/master/products.csv
wget https://raw.githubusercontent.com/tmcnab/northwind-mongo/master/categories.csv
wget https://raw.githubusercontent.com/tmcnab/northwind-mongo/master/order-details.csv
```
Neste caso, fizemos o download de todas as tuplas das 5 tabelas que iremos utilizar.

### Criação de uma sessão Spark
É necessário criar uma sessão Spark para podermos utilizar as estruturas de dados convenientes para o projeto.
```
from pyspark.sql.session import SparkSession
spark = SparkSession.builder.appName("spark_tutorial").getOrCreate()
```

### Leitura dos arquivos
O próximo passo será ler os arquivos e transformar as "tabelas" em um DataFrame, uma coleção de dados distribuídos organizados por colunas. Essa é uma estrutura de dados similar a uma tabela de um banco de dados relacional.
```
raw_orders = spark.read.option("header", True).csv("./orders.csv")
raw_customers = spark.read.option("header", True).csv("./customers.csv")
raw_products = spark.read.option("header", True).csv("./products.csv")
raw_categories = spark.read.option("header", True).csv("./categories.csv")
raw_order_details = spark.read.option("header", True).csv("./order-details.csv")
```
`spark.read` é utilizado para a leitura de arquivos.
Neste caso, usamos `option("header", True)` para que as colunas de cada tabela sejam incorporadas no DataFrame. Dessa maneira, podemos realizar operações com mais facilidade nos dados. 
Por fim, `.csv("file.csv")` denota o formato do arquivo que queremos ler. 

Para ter uma breve noção do conteúdo dos DataFrames, podemos utilizar a função `show()` e passar como argumento a quantidade de tuplas que queremos como retorno.
Por exemplo:
`raw_orders.show(3)` deverá retornar uma tabela similar a essa:
```
+-------+----------+----------+--------------------+--------------------+--------------------+-------+-------+--------------------+------------------+--------+--------------+--------------+-----------+
|OrderID|CustomerID|EmployeeID|           OrderDate|        RequiredDate|         ShippedDate|ShipVia|Freight|            ShipName|       ShipAddress|ShipCity|    ShipRegion|ShipPostalCode|ShipCountry|
+-------+----------+----------+--------------------+--------------------+--------------------+-------+-------+--------------------+------------------+--------+--------------+--------------+-----------+
|  10248|     VINET|         5|1996-07-04 00:00:...|1996-08-01 00:00:...|1996-07-16 00:00:...|      3|  32.38|Vins et alcools C...|59 rue de l'Abbaye|   Reims|          NULL|         51100|     France|
|  10249|     TOMSP|         6|1996-07-05 00:00:...|1996-08-16 00:00:...|1996-07-10 00:00:...|      1|  11.61|  Toms Spezialitäten|     Luisenstr. 48| Münster|          NULL|         44087|    Germany|
|  10250|     HANAR|         4|1996-07-08 00:00:...|1996-08-05 00:00:...|1996-07-12 00:00:...|      2|  65.83|       Hanari Carnes|       Rua do Paço|      67|Rio de Janeiro|            RJ|  05454-876|
+-------+----------+----------+--------------------+--------------------+--------------------+-------+-------+--------------------+------------------+--------+--------------+--------------+-----------+
only showing top 3 rows

```
Conseguimos vizualizar as três primeiras tuplas existente em `raw_orders`. 
Podemos fazer o mesmo com qualquer outro DataFrame para olharmos as colunas e que tipo de dados existem em cada tabela.

### Selecionando apenas as colunas necessárias
Para o nosso projeto, não iremos utilizar todos os dados existentes das tabelas que temos. Portanto, devemos criar um novo DataFrame com apenas as colunas necessárias.

Primeiramente, criamos uma **lista** para cada tabela com os nomes das colunas que desejamos manter.
```
columns_orders = ['OrderID', 'CustomerID']
columns_customers = ['CustomerID', 'CompanyName', 'Address', 'City', 'Region', 'Country']
columns_products = ['ProductID', 'ProductName', 'CategoryID', 'QuantityPerUnity', 'UnitPrice']
columns_categories = ['CategoryID', 'CategoryName', 'Description']
columns_order_details = ['OrderID', 'ProductID']
```

Depois, para cada tabela, criamos uma nova tabela que armazena apenas as colunas relevantes.
```
orders = raw_orders.select([col for col in raw_orders.columns if col in columns_orders])
customers = raw_customers.select([col for col in raw_customers.columns if col in columns_customers])
products = raw_products.select([col for col in raw_products.columns if col in columns_products])
categories = raw_categories.select([col for col in raw_categories.columns if col in columns_categories])
order_details = raw_order_details.select([col for col in raw_order_details.columns if col in columns_order_details])
```

O método `select` tem a função similar do SELECT em SQL. 
Como parâmetro do método, passamos uma *list comprehension* onde, em um laço de repetição `for`, selecionamos apenas as colunas contidas nas **listas** criadas anteriormente. 

Podemos verificar o resultado novamente utilizando o método `show()`

`orders.show(3)`

Que retornará:
```
+-------+----------+
|OrderID|CustomerID|
+-------+----------+
|  10248|     VINET|
|  10249|     TOMSP|
|  10250|     HANAR|
+-------+----------+
only showing top 3 rows
```
Antes, a tabela *Orders* continha 14 colunas. Agora contém apenas 2. Da mesma forma, filtramos todas as outras tabelas da mesma maneira selecionando apenas o que nos é relevante.

Esse é o resultado que temos:
![Clean tables](/img/clean_tables.png)

### Criando join entre tabelas
Agora que temos apenas os dados que nos é relevante para o projeto, devemos começar a agregação de dados para poder criar o documento JSON.
O primeiro passo será fazer um `join` entre as tabelas *categories* e *products*. 
A maneira mais simples de se conseguir essa junção é usar comando SQL!
Para poder consultar as tabelas usando comandos SQL, devemos inicialmente criar um buffer em memória que nos permite rodar *queries* nessas tabelas temporárias. 
```
categories.createOrReplaceTempView("tmp_categories")
products.createOrReplaceTempView("tmp_products")
``` 
Neste caso, criamos 2 tabelas temporárias. Uma para `categories` com o nome `tmp_categories` e uma para `products` com o nome `tmp_products`.

Dessa maneira conseguimos fazer o `join` nas duas tabelas e armazenar o resultado em um novo DataFrame.
```
categories_orders_join = spark.sql("SELECT tmp_categories.CategoryID, \
tmp_categories.CategoryName, tmp_categories.Description, tmp_products.ProductID, \
tmp_products.ProductName, tmp_products.UnitPrice \
FROM tmp_categories, tmp_products \
WHERE tmp_products.CategoryID ==  tmp_categories.CategoryID ORDER BY ProductID"\
)
```

Execurando `categories_orders_join.show(3)` obtemos:
```
+----------+--------------+----------------+---------+--------------+---------+
|CategoryID|  CategoryName|     Description|ProductID|   ProductName|UnitPrice|
+----------+--------------+----------------+---------+--------------+---------+
|         1|     Beverages|     Soft drinks|        1|          Chai|    18.00|
|         8|       Seafood|Seaweed and fish|       10|         Ikura|    31.00|
|         4|Dairy Products|         Cheeses|       11|Queso Cabrales|    21.00|
+----------+--------------+----------------+---------+--------------+---------+
only showing top 3 rows
```
