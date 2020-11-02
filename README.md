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







