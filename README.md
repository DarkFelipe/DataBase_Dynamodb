# DataBase_Dynamodb

Base de dados criada para estudar sobre o Dynamodb. A minha base de dados foi criada inspirada na minha colecção de CDs. <br />
Database created to study about the Dynamodb. My database was created inspired by my collection CDs.

## Informações úteis (Useful information)

Foi utilizado o prompt de comando diretamente do windows e por causa disto, precisei criar arquivos JSON para conseguir manipular o banco de dados. <br />
I used the command prompt directly from windows and because of this, i needed to create JSON files to be able to manipulate the database.


## Serviço utilizado(Service used).

- Amazon Dynamodb
- Amazon CLI para execução em linha de comando

## Comandos para execução do experimento (Commands to run the experiment)

- Criar a tabela (Create the table)

```

aws dynamodb create-table  
--table-name CollectionCDs 
--attribute-definitions  AttributeName=Artist,AttributeType=S   AttributeName=AlbumTitle,AttributeType=S 
--key-schema AttributeName=Artist,KeyType=HASH AttributeName=AlbumTitle,KeyType=RANGE 
--provisioned-throughput ReadCapacityUnits=10,WriteCapacityUnits=5

```

- Inserir um item (Insert one item)

```

aws dynamodb put-item 
--table-name CollectionCDs 
--item file://"C:\Program Files\Amazon\AWSCLIV2\src\itemalbum.json"

```

- Inserir múltiplos itens (Insert multiples items)

```

aws dynamodb batch-write-item 
--request-items file://"C:\Program Files\Amazon\AWSCLIV2\src\manyalbuns.json"

```

- Ver itens (Views items)

```

aws dynamodb scan --table-name CollectionCDs

```

- Buscar por artista (Search by artist)

```

aws dynamodb query 
--table-name CollectionCDs 
--key-condition-expression "Artist = :artist" 
--expression-attribute-values  file://"C:\Program Files\Amazon\AWSCLIV2\src\queryartist.json"

```

- Criar um index global secundário baseado na gravadora (Create a global secondary index basead on record company)

```

aws dynamodb update-table 
--table-name CollectionCDs 
--attribute-definitions AttributeName=RecordCompany,AttributeType=S 
--global-secondary-index-updates  "[{\"Create\":{\"IndexName\": \"RecordCompany-index\",\"KeySchema\":[{\"AttributeName\":\"RecordCompany\",\"KeyType\":\"HASH\"}],\"ProvisionedThroughput\": {\"ReadCapacityUnits\": 10, \"WriteCapacityUnits\": 5 },\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"

```

- Pesquisa pelo index secundário baseado na gravadora (Search by global secondary index basead on record company)

```

aws dynamodb query 
--table-name CollectionCDs 
--index-name RecordCompany-index 
--key-condition-expression "RecordCompany = :name" 
--expression-attribute-values  file://"C:\Program Files\Amazon\AWSCLIV2\src\queryRecordCompany.json"

```

- Criar um index global secundário baseado no nome do artista e no título do álbum (Create a global secondary index basead on the artist name and album title)

```
aws dynamodb update-table --table-name CollectionCDs --attribute-definitions AttributeName=AlbumTitle,AttributeType=S AttributeName=Artist,AttributeType=S --global-secondary-index-updates  "[{\"Create\":{\"IndexName\": \"ArtistAlbumTitle-index\",\"KeySchema\":[{\"AttributeName\":\"Artist\",\"KeyType\":\"HASH\"},{\"AttributeName\":\"AlbumTitle\",\"KeyType\":\"RANGE\"}],\"ProvisionedThroughput\": {\"ReadCapacityUnits\": 10, \"WriteCapacityUnits\": 5},\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"

```

- Pesquisar pelo index secundário baseado no nome do artista e no título do álbum (Search by global secondary index basead on the artist name and album title)

```

aws dynamodb query --table-name CollectionCDs --index-name ArtistAlbumTitle-index --key-condition-expression "Artist = :artist and AlbumTitle = :title" --expression-attribute-values file://"C:\Program Files\Amazon\AWSCLIV2\src\queryArtistAlbumTitle.json"

```


