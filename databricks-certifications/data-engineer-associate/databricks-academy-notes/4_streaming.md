## **Streaming:**
A configuração de gatilhos (trigger) define o momento do processamento dos dados. Se será contínuo ou em intervalos fixos.

* **_Se não especificada_:**
A consulta é executada em micro-lotes (`micro batches`), onde um lote será gerado assim que o anterior for processado completamente;

Exemplo:
```python
df.writeStream \
  .format("console") \
  .start()
```

* **_Fixa (Fixed)_**:
Também executará a query em micro-lotes, tendo um intervalo setado como referência. Considerando o micro-lote anterior:
i. **Completado no intervalo fixo**: o próximo lote esperará o tempo restante para completar o intervalo antes de ser processado;
ii. **Passar do intervalo fixo**: o próximo lote será processado assim que o anterior for finalizado, não esperando o próximo intervalo;
iii. **Sem novos dados**: o processo não se iniciará.

Exemplo:
```python
df.writeStream \
  .trigger(processingTime='5 seconds') \
  .format("console") \
  .start()
```

* **_Available-now_**: a consulta processa todos os dados disponíveis e para.

Exemplo:
```python
df.writeStream \
  .trigger(once=True) \ # legado
  .format("console") \
  .start()
```

```python
df.writeStream \
  .trigger(availableNow=True) \
  .format("console") \
  .start()
```

* **_Continuous_**: a consulta é executada continuamente, com um momento de checkpointing (intervalo em que os dados serão salvos) definido.

Exemplo:
```python
df.writeStream \
  .trigger(continuous='1 second') \
  .format("console") \
  .start()
```
