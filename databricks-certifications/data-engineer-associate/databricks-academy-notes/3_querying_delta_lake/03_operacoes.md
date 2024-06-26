### **1. Overwrite data:**
```sql
Create OR
Replace TABLE
AS SELECT
```

Ou

```sql
INSERT OVERWRITE -- ( Para tabelas existentes, com o mesmo schema, e pode se aplicar a partições individuais)
```

### **2. Append data:**
Adiciona novas linhas à uma tabela existente:
  
```sql
INSERT INTO
```

Reexecutar o insert para a mesma carga irá gerar dados duplicados

### **3. Append, update and delete:**

```sql
MERGE INTO target a
USING source b
ON {merge_condition}
WHEN MATCHED THEN {matched_action}
WHEN NOT MATCHED THEN {not_matched_action}
```

Os benefícios do merge são:
* Realizar múltiplas operações em uma transação única (update, insert e delete);
* Adicionar múltiplas condições em complemento para combinação com os campos;
* proporciona opções extensivas para implementar lógica personalizada.

Ex:
```sql
MERGE INTO users a
USING users_update b
ON a.user_id = b.user_id
WHEN MATCHED AND a.email IS NULL AND b.email IS NOT NULL THEN
	UPDATE SET email = b.email, updated = b.updated
WHEN NOT MATCHED THEN INSERT *
```

### **3.1. Lidando com deduplicação**
É comum usar o merge para lidar com deduplicação, ao utilizar somente a cláusula WHEN NOT MATCHED, como no exemplo em que adiciona os dados caso o usuário e event_timestamp não existirem na tabela:

Ex:
```sql
MERGE INTO events a
USING events_update b
ON a.user_id = b.user_id AND a.event_timestamp = b.event_timestamp    
WHEN NOT MATCHED AND b.traffic_source = ‘email’ THEN 
	INSERT *
```

### **4. Ingest data incrementally:**
Para o funcionamento da operação:
- O schema dos dados deve ser consistente;
- Duplicação dos dados deve ser tratada.

```sql
COPY INTO
```
