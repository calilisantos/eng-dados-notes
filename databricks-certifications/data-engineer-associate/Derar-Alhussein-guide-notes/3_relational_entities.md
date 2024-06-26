## Entidades Relacionais

### **Databases**
Uma estrutura lógica onde tabelas, views e funções podem ser organizadas. Todos esses objetos também pode ser chamado de schema no contexto do Databricks.<br/>
Sua criação é feita com os comandos:
```sql
CREATE DATABASE IF NOT EXISTS database_name

-- ou

CREATE SCHEMA IF NOT EXISTS database_name
```

### **Tabelas**
No Databricks, podem ser gerenciadas (**managed**), quando seus arquivos físicos são armazenados no espaço do storage separado para o Databricks, ou externas (**external**), quando os arquivos são armazenados em um local determinado na sua criação.<br/>

* **_Gerenciadas:_**
```sql
USE database_name;
CREATE TABLE table_name
```

* **_Externas:_**
```sql
USE database_name;
CREATE TABLE table_name
LOCATION 'path/to/table'
```

* **_Apagando location de tabela externa:_**
```python
dbutils.fs.rm('path/to/table', True)
```

### **CTAS (Create Table As Select)**
Cria uma nova tabela a partir de uma query.<br/>
```sql
CREATE TABLE table_name
AS SELECT * FROM source_table
```

Com outras opções:
```sql
CREATE TABLE table_name
  COMMENT 'table description'
  PARTITIONED BY (column_a, column_b)
  LOCATION 'path'
  AS SELECT column_a, column_b, column_c FROM source_table
```

### **CREATE TABLE X CTAS:**
| Tipo | Declaração de schema | População dos dados |
|------|-----------------------|---------------------|
| CREATE TABLE | Suporta declaração manual | Cria tabela vazia<br/>INSERT popula ela |
| CTAS | Inferência de schema | Criação da tabela com dados |

### **Restrição das tabelas:**
Após criada, pode ser adicionada restrições à tabela. Caso transações não atendam a restrição, a escrita na tabela falha:
```sql
ALTER TABLE table_name ADD CONSTRAINT constraint_name constraint_expression
```

### **Clone tabelas delta:**
* **_Deep clone:_** Clona a tabela e todos os seus metadados.
```sql
CREATE TABLE new_table_name
DEEP CLONE source_table_name
```

* **_Shallow clone:_** Clona somente os delta logs da tabela de origem.
```sql
CREATE TABLE new_table_name
SHALLOW CLONE source_table_name
```

### **Views**
```sql
CREATE VIEW view_name
AS <query>
```

* **_Temp view_:** São views que existem durante a spark session.
```sql
CREATE TEMP VIEW view_name
AS <query>
```

* **_Global temp view_:** Como a temp view, mas sobrevive durante o tempo que o cluster está de pé.
```sql
CREATE GLOBAL TEMP VIEW view_name
AS <query>
```

Para consultar a global temp view deve ser usada a `global_temp` database:
```sql
SELECT * FROM global_temp.view_name
```
