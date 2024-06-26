## **O que é Delta Lake (DL):**
>"Uma camada de armazenamento open source que traz confiabilidade para os data lakes por adicionar uma camada transacional de armazenamento acima dos dados armazenados em uma cloud." (Tradução livre baseada no Databricks)

No contexto do Data Lakehouse, uma **camada de armazenamento** é o *framework* responsável por gerenciar e organizar os dados armazenados em um data lake. É um intermediário pelo qual os dados são ingeridos, consultados e processados.

O Delta Lake não é um formato (como `Parquet` e `Json`, que são utilizados por ele) ou armazém dos dados em si. o `DL` é executado acima dos arquivos do lake para prover soluções para os desafios de um data lake.

### **Respostas do DL:**
Data lakes são soluções robustas para armazenamento massivo de vários formatos de dados. Apesar e por causa disso, dois desafios que enfrenta e que são endereçados pelo `DL` são:

| Desafios | Descrição | Solução(ões) |
|:--------:|---------|------------|
| **Inconsistência dos dados** | - Falta de transações ACID<br/> - Dados parcialmente comprometidos<br/> - Falha das transações (CRUD)| - Delta logs |
| **Questões de performance** | - Interação com os objetos das clouds<br/> - Variedade de formatos dos dados | - Metadados descentralizados<br/> - Padronização do formato dos arquivos |

  * **Delta logs:**
    Ou Delta Lake transaction logs, é um registro ordenado (em `json`) de cada transação na tabela desde a sua criação, funcionando como uma fonte da verdade sobre a situação da tabela e seu histórico.<br/>
    A cada consulta à tabela, o `Spark` (ou outra interface para o delta) verifica o log de transação para entender a versão mais recente do dado.<br/>
    Cada transação confirmada é gravado no log:
    * Tipo da operação (`INSERT`, `UPDATE`, ...);
    * Seus predicados (condições, filtros, ...);
    * Nome dos parquet afetados pela operação.
    Com os delta logs é garantido:
    * **Transações ACID:** O log só é gerado com a transação bem sucedida, igualmente o dado será consumido considerando a última transação<br/>
      Arquivos mal formados ou sem registro, não são considerados;
    * **Auditoria completa:** Ao registrar a operação, momento e usuário envolvido, é permitido traçar a evolução do dado e garantir sua governança e resolução de problemas.

  * **Metadados descentralizados:**
    Os metadados das tabelas são armazenados nos logs de transação ao invés de um metastore centralizado.<br/>
    No contexto de dados massivos, essa estratégia melhora a performance das consultas e leitura dos diretórios.

  * **Arquivos padronizados:**
    Ainda em performance, os arquivos físicos utilizando `Parquet` (eficientes em armazenamento e performance da consulta) e os logs em `Json` (rápidos de processar e criar), otimizam a recuperação e armazenamento dos dados.
