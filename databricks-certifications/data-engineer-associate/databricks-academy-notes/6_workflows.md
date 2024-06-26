## **Workflows**

### **O que é:**
Um serviço de orquestração de tarefas de propósito geral, totalmente gerenciado e baseado na cloud.

Nele é possível orquestrar:<br/>
i. Notebooks;<br/>
ii. SQL;<br/>
iii. Modelos de Machine Learning;<br/>
iv. Código python.

### **Serviços de orquestração:**
* **Workflow Jobs:**
	* Orquestra qualquer tipo de tarefa, podendo combinar notebooks, sql, spark, modelos de machine learning e Delta Live Tables.
	* **Recomendado para** orquestração de tarefas dependentes e agendavas, tarefas de machine learning, códigos em geral como chamadas à API, tarefas personalizadas, tarefas com arquivos jar, spark submit, python script, tarefa SQL, dbt, …
* **Delta Live Tables (DLT):**
 	* Buscar facilitar a criação de pipelines de dados batch ou streaming, usando python ou SQL, com controles e monitoramentos de qualidade integrados.
	* Só podem ser criadas em notebooks

  * **Diferenças:**
    * Workflows podem ter atividades de várias fontes, DLT apenas de notebooks;
    * DLT podem determinar as dependências necessárias para a tarefa, os jobs precisam que elas sejam informadas;
    * DLT provisiona o cluster em que sua tarefa será executada, Workflows pode provisionar um cluster ou usar um já existente;
    * DLT não oferece suporte para timeouts, com retries executando automaticamente no modo de produção. Workflows tem a função de timeout;
    * DLT não importa bibliotecas enquanto o Workflows permite essa importação

### **Padrões comuns de workflows:**
* **1.1. Sequência:** Transformação/Processamento/Limpeza de dados e Construção de tabelas medalhão
* **1.2. Funil:** Múltiplas fontes de dados; Coleções de dados
* **1.3. Espalhado:** Única fonte de dado; Ingestão e distribuição de dado
