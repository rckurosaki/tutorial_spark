## Montando um banco de dados relacional SQLite
Para montarmos o banco de dados, precisamos dos arquivos csv contendo os dados necessários. Esses arquivos podem ser encontrados ![aqui](/arquivos/csv).

Iremos utilizar um script python para criar o banco NorthWind.db.

Primeiramente, devemos importar os módulos necessários.
```
import sqlite3
import pandas as pd
```

Criamos então a conexão com o banco SQLite. Caso o banco não exista, ele criará um novo.
```
connection = sqlite3.connect('NorthWind.db')
```

Lemos, então, os dados dos arquivos csv e os armazenamos em dataframes.
Note que para cada csv, nós criamos um novo dataframe correspondente àquele arquivo. 
```
orders = pd.read_csv('./orders.csv', sep='|')
customers = pd.read_csv('./customers.csv', sep='|')
categories = pd.read_csv('./categories.csv', sep='|')
order_details = pd.read_csv('./order-details.csv', sep='|')
products = pd.read_csv('./products.csv', sep='|')
```

Por fim, cada dataframe irá se tornar uma tabela no modelo relacional. 
```
orders.to_sql('orders', connection, if_exists='replace', index=False)
customers.to_sql('customers', connection, if_exists='replace', index=False)
categories.to_sql('categories', connection, if_exists='replace', index=False)
order_details.to_sql('order_details', connection, if_exists='replace', index=False)
products.to_sql('products', connection, if_exists='replace', index=False)
```
Neste caso, o dataframe chama um método para converter o dataframe em uma tabela sql.

E pronto! Temos o nosso banco de dados SQLite!
Como pode ver, os módulos  `sqlite3` e `pandas` facilitaram muito o nosso trabalho, abstraindo muito das complexidades que poderíamos encontrar ao criar um novo banco de dados.

