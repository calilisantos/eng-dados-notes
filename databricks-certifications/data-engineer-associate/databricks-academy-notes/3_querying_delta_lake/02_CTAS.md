### **1. O que é:**
Comando para criar uma tabela à partir de uma query ou comando em atividade.
```sql
CREATE
TABLE
AS
SELECT ...
```

Ex:
```sql
CREATE OR REPLACE TABLE table_name AS
SELECT * FROM some_table;
```

### 2. Clones:

**2.1 DEEP CLONE:** Copia os dados e metadados de uma tabela alvo para um destino. A cópia acontece incrementalmente, então a cada execução do comando as mudanças na origem são refletidas no destino

**2.2. SHALLOW CLONE:** Só copia os logs de transação da tabela de origem, sem mover os dados da origem. Mais rápida que o comando anterior.