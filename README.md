# MongoDB
> Resumo ─ Módulo Persistência de Dados, Versionamento e Implantação ─ Bootcamp Desenvolvedor FullStack da IGTI

O **MongoDB** é um banco de dados NoSQL orientado à objetos que vem sendo adotado tanto em startups quanto em pequenas e grandes coorporações.

A organização dos dados no MongoDB é definida conforme a hierarquia abaixo:

- **Banco de dados:** pode conter uma ou mais coleções;
- **Coleções:** pode conter diferentes tipos de documentos;
- **Documentos:** pode conter tuplas de chave e valor em lista, vetor ou uma
referência à um documento.
- **Campos:** tuplas, vetores ou referências a outros documentos

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

### Base exemplo
> Database: notas  
> Collection: aluno

|_id|nome|sexo|nota|
|--|--|--|--|
|1|João Paulo|M|9|
|2|Maria dos Anjos|F|7.5|
|3|Ana Julia|F|10|
|4|Joaquim Monteiro|M|6.3|

### Comandos básicos
- `mongod --dbpath="X:\Exemplo"`: define um diretório para armazenamento dos dados;
- `mongo -host localhost:27017`: acesso ao banco local;
- `use notas`: acessar uma base de dados, caso a mesma não exista, automaticamente é criada após a inserção de um documento na base citada;
- `show dbs`: exibe as bases de dados existentes;
- `db`: exibe a base que está sendo manipulada;
- `db.dropDatabase()`: remove a base de dados atual do servidor MongoDB;

### Coleções
- `db.createCollection(name, options)`: criar uma coleção na base de dados;
  - `name`: nome da coleção;
  - `options (opcional)`: define as configurações da coleção.
    |Campo|Tipo|Descrição|
    |--|--|--|
    |capped|boolean|Define uma coleção limitada, se `true`, o campo size deve ser definido|
    |size|number|Tamanho em `bytes` para definir o limite da coleção|
    |max|number|Define o limite máximo de documento na coleção|
    |validator|document|Documento que define regras e exceções da documentos|
    |validationLevel|string|Define o rigor das regras de validação aplicadas aos documentos|
    |validationAction|string|Determina se acusa erros em documentos inválidos ou apenas alerta sobre as violações (Permite documentos inválidos serem inseridos)|
- `show collections`: lista as coleções existentes;
- `db.aluno.drop()`: remove a coleção da base de dados.

### Create
- `db.aluno.insertOne({nome: "Ana Julia", sexo: "F", nota: 10})`: insere apenas um documento na base de dados;
- `db.aluno.insertMany([{nome: "Maria dos Anjos", sexo: "F", nota: 7.5}, {nome: "João Paulo", sexo: "M", nota: 9}])`: insere múltiplos documentos objeto na base de dados;
- `db.aluno.insert({...})`: insere um ou mais documento na base de dados;

### Retrieve
- `db.aluno.find(query, porjection)`: retorna todos os documentos existentes na coleção;
  - `query (opcional)`: quais filtros serão utilizados;
    ```shell
      db.aluno.find({nome: "Ana Julia"}, {});
      # Output:
      # { "_id" : 1, "nome" : "Ana Julia", "sexo" : "F", "nota" : 10 }
    ```
  - `projection (opcional)`: quais campos serão retornados (0: remove | 1: permanece).
    ```shell
      db.aluno.find({}, {_id: 0, nome: 1});

      # Output:
      # { "nome" : "Ana Julia" }
      # { "nome" : "Maria dos Anjos" }
      # { "nome" : "João Paulo" }
      # { "nome" : "Joaquim Monteiro" }
    ```
  - `.limit(n)`: adiciona o limite de documentos que serão retornados;
  - `.skip(n)`: pula alguns documentos na consulta;
  - `.sort({_id: 1})`: ordena os valores da consulta de acordo com o campo passado (-1: decrescente | 1: crescente)
  - `pretty()`: exibe o resultado de forma mais organizada;
- `db.aluno.findOne(query, projection)`: retorna apenas um registro;
- Operadores lógicos:
  - `.find({$and: [{nome: ...}, {nota: ...}]})`: Retorna tudo que atende as condições;
  - `.find({$or: [{sexo: ...}, {nota: ...}]})`: Retorna tudo que atende uma das confições;
  - `.find({sexo: {$not: {$eq: "F"}}});`: Inverte a condição especificada;
  - `.find({$nor: [{nome: ...}, {nota: ...}]})`: Retorna tudo que não atender as condições;
- Operadores de comparação:
  - `$eq`: Igual à ...
    ```
    db.aluno.find({nota: {$eq: 10}}, {_id: 0});
    ```
  - `$gt`: Maior que ...
    ```
    db.aluno.find({nota: {$gt: 5}}, {_id: 0});
    ```
  - `$gte`: Maior ou igual ...
    ```
    db.aluno.find({nota: {$gte: 7}}, {_id: 0});
    ```
  - `$in`: Pertence à ...
    ```
    db.aluno.find({nome: {$in: ["Ana", "João"]}}, {_id: 0});
    ```
  - `$lt`: Menor que ...
    ```
    db.aluno.find({nota: {$lt: 5}}, {_id: 0});
    ```
  - `$lte`: Menor ou igual ...
    ```
    db.aluno.find({nota: {$lte: 7}}, {_id: 0});
    ```
  - `$ne`: Não é igual ...
    ```
    db.aluno.find({sexo: {$ne: "M"}}, {_id: 0});
    ```
  - `$nin`: Não pertence à ...
    ```
    db.aluno.find({nome: {$nin: ["Joaquim, "Maria"]}}, {_id: 0});
    ```

## Autor
Aryosvalldo Cleef ─ [linkedin](https://www.linkedin.com/in/aryosvalldo-cleef/) ─ [@cleefsouza](https://github.com/cleefsouza)

## Meta
Made with 💚 by **Cleef Souza**
