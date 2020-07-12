# MongoDB
> Resumo ─ Módulo Persistência de Dados, Versionamento e Implantação ─ Bootcamp Desenvolvedor FullStack da IGTI

O **MongoDB** é um banco de dados NoSQL orientado à objetos que vem sendo adotado tanto em startups quanto em pequenas e grandes coorporações.

A organização dos dados no MongoDB é definida conforme a hierarquia abaixo:

- **Banco de dados:** pode conter uma ou mais coleções;
- **Coleções:** pode conter diferentes tipos de documentos;
- **Documentos:** pode conter tuplas de chave e valor em lista, vetor ou uma
referência à um documento;
- **Campos:** tuplas, vetores ou referências a outros documentos.

<img src="https://d2m498l008ebpa.cloudfront.net/2017/07/sql-nosql.png" title="BDR vs MongoDB" width=500/>

### Download e instalação
- Realize o download da versão community no site do MongoDB;
  > **Link:** [MongoDB Community Server](https://www.mongodb.com/try/download/community)
- Para facilitar o manuseio do mongo no windows, adicione as variaveis de ambiente o **path** da pasta bin;
- Verifique se está tudo funcionando executando o comando `mongo --version` no seu terminal. Exemplo do que deve ser exibido:
  ```shell
  MongoDB shell version v4.2.8
  git version: 43d25964249164d76d5e04dd6cf38f6111e21f5f
  OpenSSL version: OpenSSL 1.1.1  11 Sep 2018
  allocator: tcmalloc
  modules: none
  build environment:
      distmod: ubuntu1804
      distarch: x86_64
      target_arch: x86_64
  ```

### Comandos básicos
- Definir um diretório para armazenamento dos dados;
  ```shell
  mongod --dbpath="X:\...\Exemplo"
  ```
- Acessar o banco localmente;
  ```shell
  mongo -host localhost:27017
  ```
- Acessar uma base de dados, caso a mesma não exista, automaticamente é criada após a inserção de um documento na base citada;
  ```shell
  use DATABASE
  ```
- Exibir as bases de dados existentes;
  ```shell
  show dbs
  ```
- Exibir a base que está sendo manipulada;
  ```shell
  db
  ```
- Remover a base de dados atual do servidor MongoDB.
  ```shell
  db.dropDatabase()
  ```

### Coleções
- Criar uma coleção na base de dados;
  ```shell
  db.createCollection(name, options);
  ```
  - `name`: nome da coleção;
  - `options (opcional)`: define as configurações da coleção;
    |Campo|Tipo|Descrição|
    |--|--|--|
    |capped|boolean|Define uma coleção limitada, se `true`, o campo size deve ser definido|
    |size|number|Tamanho em `bytes` para definir o limite da coleção|
    |max|number|Define o limite máximo de documento na coleção|
    |validator|document|Documento que define regras e exceções da documentos|
    |validationLevel|string|Define o rigor das regras de validação aplicadas aos documentos|
    |validationAction|string|Determina se acusa erros em documentos inválidos ou apenas alerta sobre as violações (Permite documentos inválidos serem inseridos)|
- Listar as coleções existentes;
  ```shell
  show collections;
  ```
- Remover uma coleção da base de dados.
  ```shell
  db.COLLECTION.drop();
  ```

### Criar
- Inserir apenas um documento na base de dados;
  ```shell
  db.COLLECTION.insertOne({nome: "Exemplo 1", tipo: 1, valor: 10});
  ```
- Inserir múltiplos documentos objeto na base de dados;
  ```shell
  db.COLLECTION.insertMany([
    {nome: "Exemplo 2", tipo: 2, valor: 7.5},
    {nome: "Exemplo 3", tipo: 3, valor: 9}
  ]);
  ```
- Inserir um ou mais documento na base de dados.
  ```shell
  db.COLLECTION.insert({...});
  ```

### Consultar
- Retorna todos os documentos existentes na coleção;
  ```shell
  db.COLLECTION.find(query, porjection);
  ```
  - `query (opcional)`: quais filtros serão utilizados;
    ```shell
    db.COLLECTION.find({nome: "Exemplo 1"}, {});

    # Output:
    # { "_id" : "5f022654f66a26475d4fbf8c", "nome" : "Exemplo 1", "tipo" : "1", "valor" : 10 }
    ```
  - `projection (opcional)`: quais campos serão retornados;
    ```shell
    # 0: remove
    # 1: permanece

    db.COLLECTION.find({}, {_id: 0, nome: 1});

    # Output:
    # { "nome" : "Exemplo 1" }
    # { "nome" : "Exemplo 2" }
    # { "nome" : "Exemplo 3" }
    ```
  - Adicionar limite de documentos que serão retornados;
    ```shell
    db.COLLECTION.find().limit(n);
    ```
  - Pular alguns documentos na consulta;
    ```shell
    db.COLLECTION.find().skip(n);
    ```
  - Ordenar os valores da consulta de acordo com o campo passado;
    ```shell
    # 1: crescente
    # -1: decrescente

    db.COLLECTION.find().sort({_id: 1});
    ```
  - Exibir os resultados de forma mais organizada;
    ```shell
    db.COLLECTION.find().pretty();
    ```
- Retornar apenas um registro.
  ```shell
  db.COLLECTION.findOne(query, projection);
  ```

### Operadores lógicos
  - Retorna tudo que atende as condições;
    ```shell
    # $and

    db.COLLECTION.find({$and: [{nome: ...}, {valor: ...}]});
    ```
  - Retorna tudo que atende uma das confições;
    ```shell
    # $or

    db.COLLECTION.find({$or: [{tipo: ...}, {valor: ...}]});
    ```
  - Inverte a condição especificada;
    ```shell
    # $not

    db.COLLECTION.find({tipo: {$not: {$eq: "1"}}});
    ```
  - Retorna tudo que não atender as condições.
    ```shell
    # $nor

    db.COLLECTION.find({$nor: [{nome: ...}, {valor: ...}]});
    ```

### Operadores de comparação
  - Igual à ...
    ```shell
    # $eq

    db.COLLECTION.find({tipo: {$eq: 10}}, {_id: 0});
    ```
  - Maior que ...
    ```shell
    # $gt
    
    db.COLLECTION.find({tipo: {$gt: 5}}, {_id: 0});
    ```
  - Maior ou igual ...
    ```shell
    # $gte
    
    db.COLLECTION.find({tipo: {$gte: 7}}, {_id: 0});
    ```
  - Pertence à ...
    ```shell
    # $in
    
    db.COLLECTION.find({nome: {$in: ["Exemplo 4", "Exemplo 5"]}}, {_id: 0});
    ```
  - Menor que ...
    ```shell
    # $lt
    
    db.COLLECTION.find({tipo: {$lt: 5}}, {_id: 0});
    ```
  - Menor ou igual ...
    ```shell
    # $lte
    
    db.COLLECTION.find({tipo: {$lte: 7}}, {_id: 0});
    ```
  - Não é igual ...
    ```shell
    # $ne
    
    db.COLLECTION.find({tipo: {$ne: 3}}, {_id: 0});
    ```
  - Não pertence à ...
    ```shell
    # $nin
    
    db.COLLECTION.find({nome: {$nin: ["Exemplo 1", "Exemplo 2"]}}, {_id: 0});
    ```

### Atualizar
- Atualiza um documento especifico em uma coleção;
  - `query`: busca a primeira ocorrencia do documento de acordo com os parâmetros na query;
  - `update`: quais os novos valores que serão modificados;
  ```shell
  db.COLLECTION.updateOne({nome: "Exemplo 1"}, {$set: {tipo: 2, valor: 5.99}});
  ```
- Atualiza varios documentos de uma coleção de acordo com os parâmetros na query;
  ```shell
  # Atualizando todos os documentos do tipo 2

  db.COLLECTION.updateMany({tipo: 2}, {$set: {valor: 2.50}});
  ```
- Sobrescreve todo o documento por um novo;
  ```shell
  # Sobrescrevendo o documento de id 1 por um novo documento

  db.COLLECTION.replaceOne({_id: ObjectId("5f022654f66a26475d4fbf8c")}, { nome : "Exemplo 99", tipo : 99, valor : 99 });
  ```
- Modifica um ou mais documentos em uma coleção.
  ```shell
  db.COLLECTION.update(query, update, options);
  ```

### Operadores de atualização
  - Incrementa o valor atual de um campo;
    ```shell
    # $inc
    # Incrementando 10 ao valor atual

    db.COLLECTION.updateOne({nome: "Exemplo 1"}, {$inc: {valor: 10}});
    ```
  - Atualiza o valor de um campo ou cria um novo campo;
    ```shell
    # $set

    db.COLLECTION.updateOne({nome: "Exemplo 1"}, {$set: {tipo: 5, valor: 5.99}});
    ```
  - Atualiza um campo data com a data e hora atual;
    ```shell
    # $currentDate
    # Adicionando o campo modificacao com a data e hora atual em todos os documentos da coleção

    db.COLLECTION.updateMany({}, {$currentDate: {modificacao: true}});
    ```
  - Utilizado para remover campos de um documento;
    ```shell
    # $unset
    # Removendo campo modificacao de todos os documentos da coleção

    db.COLLECTION.updateMany({}, {$unset: {modificacao: 1}});
    ```
  - Multiplica o valor de um campo.
    ```shell
    # $mul
    # Multiplicando valor atual por 2

    db.COLLECTION.updateOne({nome: "Exemplo 1"}, {$mul: {valor: 2}});
    ```

### Deletar
- Deleta a primeira ocorrencia do documento de acordo com os parâmetros na query;
  ```shell
  # Deletar o documento cujo id é 5f022654f66a26475d4fbf8c

  db.COLLECTION.deleteOne({_id: ObjectId("5f022654f66a26475d4fbf8c")});
  ```
- Deletar vários documentos de acordo com o filtro estabelecido.
  ```shell
  # Deletar os documentos cujo tipo é 3

  db.COLLECTION.deleteMany({tipo: 3});
  ```

### BulkWrite
Executar comandos em massa.

```shell
db.COLLECTION.bulkWrite([operacao1, operacao2, ...], options);
```

```shell
# Exemplo de uma execução comandos em massa

db.COLLECTION.bulkWrite(
  [
    {
      insertOne: {
        "document": {
          nome: "Exemplo 1",
          tipo: 1,
          valor: 5.99
        }
      }
    },
    {
      insertOne: {
        "document": {
          nome: "Exemplo 2",
          tipo: 2,
          valor: 3.25
        }
      }
    },
    {
      updateOne: {
        "filter": {
          nome: "Exemplo 3",
          tipo: 3
        },
        "update": {
          $set: {
            valor: 9.85
          }
        }
      }
    },
    {
      insertOne: {
        "document": {
          nome: "Exemplo 4",
          tipo: 2,
          valor: 1.99
        }
      }
    },
    {
      deleteOne: {
        "filter": {
          _id: ObjectId("5f0520e6b7407942726e4ce0")
        }
      }
    },
    {
      ...
    }
  ], {ordered: true});
```

### Índices
Estruturas de dados especiais que armazenam informações de um ou mais campos com um determinada ordenação. Facilitam e melhoram a busca por seus documentos.
- Exibe os índices criados de uma determinada coleção;
  ```shell
  db.COLLECTION.getIndexes()
  ```
- Criando índice na coleção;
  ```shell
  # 1: crescente
  # -1: decrescente

  db.COLLECTION.createIndex({tipo: 1});

  # "text": índice do tipo text

  db.COLLECTION.createIndex({nome: "text"});

  # Busca utilizando índice "text"

  db.COLLECTION.find({$text: {$search: "Exemplo"}}, {_id: 1});
  ```
- Removendo índice da coleção.
  ```shell
  # Um índice específico

  db.COLLECTION.dropIndex({nome: -1});

  # Todos os índices

  db.COLLECTION.dropIndexes();
  ```


### Mais informações
Para mais detalhes sobre o MongoDB e sua gama de funcionalidades, consulte a documentação em [MongoDB Getting Started](https://docs.mongodb.com/manual/tutorial/getting-started/).

### Autor
Aryosvalldo Cleef ─ [linkedin](https://www.linkedin.com/in/aryosvalldo-cleef/) ─ [@cleefsouza](https://github.com/cleefsouza)

### Meta
Made with 💚 by **Cleef Souza**
