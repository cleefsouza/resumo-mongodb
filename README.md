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

### Comandos básicos
- `mongod --dbpath="X:\Exemplo"`: define um diretório para armazenamento dos dados;
- `mongo -host localhost:27017`: acesso ao banco local;
- `use notas`: acessar uma base de dados, caso a mesma não exista, automaticamente é criada após a inserção de um documento na base citada;
- `show dbs`: exibe as bases de dados existentes;
- `db.aluno.insert({nome: "Maria dos Anjos"})`: insere um documento na base de dados;
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
- `db.aluno.insertOne({nome: "Maria dos Anjos"})`: insere apenas um documento na base de dados;
- `db.aluno.insertMany([{nome: "Maria dos Anjos", sexo: "F"}, {nome: "João Paulo", sexo: "M"}])`: insere múltiplos documentos objeto na base de dados;
- `db.aluno.insert({...})`: insere um ou mais documento na base de dados;


## Autor
Aryosvalldo Cleef ─ [linkedin](https://www.linkedin.com/in/aryosvalldo-cleef/) ─ [@cleefsouza](https://github.com/cleefsouza)

## Meta
Made with 💚 by **Cleef Souza**
