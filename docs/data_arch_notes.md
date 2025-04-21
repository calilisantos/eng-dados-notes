## Modelo dimensional de warehouse:
Composto por dimensão e tabela.
* Dimensões: propriedades da `tabela->dimensão` que podem mas raramente são alterados
* Fato: métricas e informações que são frequentemente alterados na dimensão. Geralmente numéricos

### Estruturas warehouse:
* Star schema: Fatos no centro, dimensões nas pontas da estrela;
* Snow flake schema: Fatos no centro com dimensões semelhantes ao 3º nível de normalização

### Desvantagem:
* Inserir coluna:
  * Slow change dimensions (SCD):
    * Tipo 1: Update na dimensão. Não armazena informação anterior;
    * Tipo 2: Mantém o registro antigo e insere o novo através de um chave forte (fato)
   
## Lakehouse:
Modelo mais usado no contexto de lakes atualmente. Traz o warehouse para o lake.
