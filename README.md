# Prática

### Projeto
Para a parte prática do tutorial, iremos montar um projeto que implementa um ETL (**E**xtract, **T**ransform, **L**oad). Isto é, iremos extrair os dados de uma fonte, realizaremos o processamento desses dados e depois iremos armazená-los em outro lugar.
A ideia será extrair os dados que estão em um modelo relacional, fazer uma agregação para transformar em um modelo de documento e salvar em um arquivo JSON.
Esse projeto irá utilizar os dados da *[Northwind sample database](https://docs.yugabyte.com/latest/sample-data/northwind/)*. 
Essa base de dados contém dados de vendas de uma companhia fictícia chamada *Northwind Traders*. Essa empresa importa e exporta produtos alimentícios por todo o mundo.


### A base de dados
A base de dados completa contém 14 tabelas em um modelo relacional.  
![Diagrama base de dados](https://raw.githubusercontent.com/rckurosaki/tutorial_spark/main/img/diagrama_completo.png?token=AJN5KST36AILIXHSRPCNJTC7UBCKE)
Para o nosso projeto, apenas utilizaremos 5 dessas tabelas. 

![Tabelas utilizadas no projeto](https://raw.githubusercontent.com/rckurosaki/tutorial_spark/main/img/diagrama_blur.png?token=AJN5KSRAZOM5I4A62X6OS6K7UBCPA)
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







