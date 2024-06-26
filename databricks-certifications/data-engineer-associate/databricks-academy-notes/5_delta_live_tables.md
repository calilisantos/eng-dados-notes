## **Delta Live Tables (DLT)**
Tem o objetivo de facilitar a criação de pipelines batch e streaming com SQL e Python.


### **Live Tables:**
Uma view materializada para o lakehouse.

* Definida por uma query SQL;
* Criada e atualizada por um pipeline Delta Live Table.

Suas ferramentas:
* **Gerenciar dependências** entre tabelas;
* **Controle de qualidade** de dados;
* **Automatizar operaçõe**s de ETL;
* **Simplificar operações**;
* **Economizar custos**;
* **Reduzir latência**.

Sintaxe:
```sql
CREATE LIVE TABLE <table_name>
AS SELECT <query>
FROM <source_table>
```

### **Streaming Live Tables:**
Uma live table com "estado" que é atualizada continuamente:
* Garante exatamente um processamento de adicionar linhas;
* Inserções são lidas uma única vez;
* Permitem redução de custos e latência ao reprocessar dados antigos.

Sintaxe:
```sql
CREATE STREAMING LIVE TABLE <table_name>
AS SELECT <query>
FROM <source_table>
```

### **Etapas da Live Table Pipelines:**
* Escrita da `CREATE LIVE TABLE` em um notebook;
* Criação de um pipeline Delta Live Table;
* Execução do pipeline (botão Start).

### **DEVxPRD pipelines:**
* **DEV:** Reusa um cluster em execução para interação rápida e sem retry nos erros, possibilitando a depuração rápida;
* **PRD:** Redução de custos ao desligar os clusters assim que finalizados.
Escalonamento dos retries incluindo restart dos clusters, garantindo confiabilidade nas transações.

### **Live dependencies:**
- Tabelas do catálogo são lidas normalmente;
- Tabelas criadas na mesma pipeline são lidas do schema LIVE;
- DLT detecta essas dependências e executa as operações na ordem correta;
- DLT lida com paralelismo e captura a linhagem das tabelas.

Exemplo:
```sql
CREATE LIVE TABLE events

CREATE LIVE TABLE events_summary
AS SELECT COUNT(*) FROM LIVE.events
```

### **Expectativas:**
São testes que garantem data quality em produção. Ela valida todos os registros da carga da pipeline oferecendo como políticas para violação de expectations:
* **_Registrar_** o número de registros ruins;
* **_Deletar_** registros ruins;
* **_Abortar_** o processamento dos registros ruins.

Exemplo:
```sql
CONSTRAINT valid_date
EXPECT (date> "2020-01-01")
ON VIOLATION DROP
```

ou

```python
dlt.expect_or_drop(
  "valid_date",
  col("date") > "2020-01-01"
)
```

### **Pipeline UI:**
Permite:<br/>
i. **Visualizar** fluxo;<br/>
ii. **Descobrir** metadados e quality de cada tabela;<br/>
iii. **Acessar** histórico de atualizações;<br/>
iv. **Controlar** operações;<br/>
v. **Investigar** os eventos.

### **Streaming:**
Possibilita ingerir arquivos assim que são carregados no storage.
Com o `cloud_files` é feita a deduplicação dos dados através do `Auto Loader`.

Exemplo:
```sql
CREATE STREAMING LIVE TABLE events
AS SELECT * FROM cloud_files("path", "json")
```

Com o comando `STREAM` é feito o stream de qualquer tabela delta.

Exemplo:
```sql
CREATE STREAMING LIVE TABLE events
AS SELECT * FROM STREAM(source_table)
```

Com o `STREAM`:
* São lidos novos registros;
* Devem ser tabelas `append-only`;
* Qualquer tabela `append-only` pode ser usada como fonte.