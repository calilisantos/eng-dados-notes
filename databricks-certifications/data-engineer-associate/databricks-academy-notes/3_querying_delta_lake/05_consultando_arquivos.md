* **Usando o formato text para carregar arquivos:**
```sql
SELECT * FROM text.`${path_do_arquivo}`
```

Irá carregar cada linha do arquivo como uma linha string da coluna value de um dataframe spark.(Vale para json, csv, txt,…)<br/>
Pode ser útil para dados potencialmente corrompidos e textos personalizados.


* **Carregando todo o arquivo ou seus metadados:**
Em contextos de trabalho com imagens ou dados não estruturados, o `binaryFile` permite carregar a representação binária de todo um arquivo.

Especificamente, os campos criados indicarão: **_path, modificationTime, length e content_**.

```sql
SELECT * FROM binaryFile.`${path_do_arquivo}
```
