# DataBase_Dynamodb

Base de dados criada para estudar sobre o Dynamodb. A minha base de dados foi criada inspirada na minha colecção de CDs.
Database created to study about the Dynamodb. My database was created inspired by my collection CDs.


## Serviço utilizado(Service used).

- Amazon Dynamodb
- Amazon CLI para execução em linha de comando

## Comandos para execução do experimento (Commands to run the experiment)

- Criar a tabela (Create the table)

```

aws dynamodb create-table  --table-name CollectionCDs --attribute-definitions  AttributeName=Artist,AttributeType=S   AttributeName=AlbumTitle,AttributeType=S --key-schema AttributeName=Artist,KeyType=HASH AttributeName=AlbumTitle,KeyType=RANGE --provisioned-throughput ReadCapacityUnits=10,WriteCapacityUnits=5

```

- Inserir um item

```


