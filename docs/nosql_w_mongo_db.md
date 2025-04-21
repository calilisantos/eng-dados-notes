## NoSQL c/ MongoDB
* O que é: NoSQL são conjuntos de dados sem relações (FK, 1:1, 1:N)

### Estrutura de armazenamento 
* Diversos bancos:
  Diferente do SQL, o banco e a coleção podem ser criados (caso não existam) juntos com o documento inserido. A atualização também, ocorre com a inserção.

  * Ex:
    ```mongo
      use nomeDoBanco
      db.nomeDaColecao.insertOne({x:1})
    ```
* Coleções nos bancos (equivalentes às tabelas SQL)
  Caso aja a necessidade de especificar parâmetros na coleção, com o tamanho máximo ou regras de validação dos documentos, existe o método `db.createCollection()`
  * Ex:
    ```mongo
      db.createCollection("nomeDaColecao", {Collation: {locale:"pt"}});
    ```
    * Collation: documento do mongoDB usada para especificar regras dos documentos da coleção.
* Documentos nas coleções (equivalente aos registros [linhas] SQL)
  As coleções são schemaless (sem schema padrão), assim ela pode incluir documentos com estruturas diferentes

  Dessa forma o CRUD deve ser feito por documento para não alterar estruturas diferentes entre documentos

* Documentos são armazenados no formato `bson` (binary json)

### Classificações NoSQL:
1. Chave-valor: Redis (usado pelo twitter para lidar com cache);
2. Família de colunas: Cassandra (instagram para lidar com dm's);
3. Grafos: neo4j (Ebay para pesquisa de produtos);
4. Documentos: mongoDB.

### Interfaces:
* VsCode (extensão mongoDB);
* MongoDB Compass

### Métodos:
* `.find({query}, {projection})`
  * query: semelhante o where do SQL;
  * projection: semelhante ao `attributes` dos ORMs como Sequelize/Prisma.

