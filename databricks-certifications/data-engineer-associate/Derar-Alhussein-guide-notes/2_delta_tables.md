## Trabalhando com Tabelas Delta:

### **Criando Tabelas Delta:**
```sql
CREATE TABLE table_name(
  column_name data_type,
  ...
)
USING DELTA; -- Opcional no Databricks, que por padrão já cria tabelas Delta.
```

### **Inserindo Dados:**
* **_Uma linha_:**
```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

* **_Mais de uma linha_:**
```sql
INSERT INTO table_name (column1, column2, ...)
VALUES
  (value1, value2, ...),
  (value1, value2, ...),
  ...
```

Cada operação de `INSERT` representa uma transação que impactará a tabla.<br/>
Se os dois comandos acima forem executados para a mesma tabela, um arquivo armazenará o primeiro `INSERT`, e o segundo `INSERT` adicionará N registros em um segundo arquivo.

### **Atualizando Dados:**
Os arquivos delta são imutáveis. Com o update, um novo arquivo é criado copiando o anterior e aplicando as alterações.
```sql
UPDATE table_name
SET column1 = column1 + 1
WHERE id = 51;
```

### **Delta Time Travel:**
Por conta do `delta_log`, é possível consultar um versão específica de uma tabela delta ou restaurá-la.

* **_Consulta por timestamp:_**
```sql
SELECT * FROM table_name TIMESTAMP AS OF '2021-01-01T00:00:00.000Z';
```

* **_Consulta por versão:_**
```sql
SELECT * FROM table_name VERSION AS OF 5;

-- ou

SELECT * FROM table_name@v5;
```

* **_Restaurando por timestamp:_**
```sql
RESTORE TABLE table_name TO TIMESTAMP AS OF '2021-01-01T00:00:00.000Z';
```

* **_Restaurando por versão:_**
```sql
RESTORE TABLE table_name TO VERSION AS OF 5;
```

### **Otimizando Tabelas Delta:**
Delta tables tem um comando específico para compactar `small files`, particularmente útil para melhorar a velocidade das consultas:
```sql
OPTIMIZE table_name;
```

Uma extensão do `OPTIMIZE` é o comando `ZORDER`. Ele soma à otimização o ordenamento dos registros dos arquivos resultantes da compactação, baseado em uma ou mais colunas:
```sql
OPTIMIZE table_name 
ZORDER BY column_name;
```

Se a coluna for numérica e seja usada como filtro de uma consulta, o delta lake consegue identificar rapidamente o arquivo que contém aquela posição e procurar o índice naquele arquivo ao invés de ter que ler todo o diretório.<br/>
Como na atualização dos dados, por conta da imultabilidade do delta, o `OPTIMIZE` cria novos arquivos com os dados compactados.

### **VACUUM:**
É um mecanismo delta para gerenciar arquivos de ddos inutilizados. Por padrão o `VACUUM` só performa em dados mais velhos do que 7 dias. Sua sintaxe é:
```sql
VACUUM table_name RETAIN 0 HOURS; -- para quantidade específica de horas

-- ou

VACUUM table_name; -- para a quantidade padrão
```

Importante que após o `VACUUM` o `time travel` não é possível para os arquivos apagados.

### **Apagando Dados:**
```sql
DROP TABLE table_name;
```
