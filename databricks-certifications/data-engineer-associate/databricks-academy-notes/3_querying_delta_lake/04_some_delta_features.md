**1. Time travel:**

```sql
RESTORE TABLE table_name TO VERSION AS OF version_number (restaurar versão da tabela)

SELECT * FROM table_name VERSION AS OF version_number (consulta versão da tabela)
```

**2. Purge history:**

```sql
VACUUM table_name RETAIN 0 HOURS DRY RUN -- (mantém apenas a versão atual da tabela, e com o DRY RUN são impressos os registros a serem deletados
```
