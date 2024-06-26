* **Dados aninhados:**
	Na API Dataframe do Spark, é possível consumir dados aninhados com a seguinte sintaxe:
    
  ```python
	df.where(“coluna_aninhada:chave_aninhada =    ‘nome_da_chave’”)
	```
	ou
  ```sql
	SELECT * FROM df WHERE coluna_aninhada:chave_aninhada =    ‘nome_da_chave’
  ```

* **Obtendo schema de uma coluna com valores contendo uma json string:** 

```python
schema_of_json()
```

* **Convertendo uma coluna json string para um tipo estruturado à partir de um schema:**

```python
from_json()
```

* **Separando elementos de uma coluna de array em múltiplas colunas:** 

```python
explode()
```

* **Contando o número de elementos do array dessas colunas:** 

```python
size()
```

* **Valores únicos em structtypes:**

```python
collect_set() # coleta valores únicos de um campo, incluindo valores de arrays
array_distinct() # remove elementos duplicados de um array
```

* **Combinando múltiplos arrays:**

```python
flatten()
```
Pandas UDF usa Apache Arrow e é mais performática do que a python pdf pura

* **UDFs:**
Pandas UDF usa Apache Arrow e é mais performática do que a python pdf pura.
