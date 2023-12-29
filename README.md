# **Engenharia de dados:**

## **1. O que é:**
É a capacidade responsável pelo processamento (ETL/ELT) e camadas analíticas de um time de dados.

### **1.1. Objetivo:**
Buscar a solução ótima para o problema de negócio, considerando a relação entre performance e custo.

### **1.2. Problema fundamental:**

i. Ter o menor tempo de processamento;

ii. Para o maior volume de dados;

iii. Com o menor custo possível.

Obs: Custos não são apenas financeiros, mas também esforço, tempo, capacidade técnica, etc.

### **1.3. Especialidades:**

- **Plataforma**: Habilitam a engenharia de dados com o desenvolvimento e definições de uso principalmente das ferramentas de processamento de dados;

- **DataOps**: Habilitam a engenharia de dados com o desenvolvimento e definições de uso principalmente da infraestrutura (ambientes e camadas analíticas) de dados;

- **Enablers**: Aplicam o processamento dos dados, utilizando as ferramentas e infraestrutura disponibilizadas pela plataforma nas demandas de negócio em especial de analytics;

- **MLOps**: Tem funções semelhantes a Plataforma e DataOps, porém com foco em Machine Learning e os times de Data Science.

## **2. Processamento de dados:**

Compreende a extração, transformação e carga de dados (Extract, Transform, Load não necessariamente nessa ordem) de um ou mais sistemas de origem para um ou mais sistemas de destino.
* **Ingestão. `E` do ETL:**
É a etapa de construção de um fluxo de extração de dados, seja manual ou automatizada, de determinada fonte de dados.
Tem diversos tipos e outras classificações, os mais importantes são:

  * **Lote** (batch): É a extração com início e fim definidos. Poderia e deveria ser a mais comum.

    Poderia porque a maioria das demandas de negócio não precisam de dados em tempo real.
    
    Deveria porque tem menor custo de manutenção e implementação em regra.
  * **Contínuo** (Streaming): É a extração contínua de dados (não me diga!) tendo duas variações principais:
    * **Microbatch**: É streaming mas feita em pequenos lotes geralmente em pequenos intervalos de tempo e disparada por eventos, com um custo e desafios maiores que o lote.
    * **_Near real time_**: O dado é extraído em tempo real, mas processado _on demand_. Só é limitada pela capacidade de processamento.

* **Transformação:**
É a etapa de transformação dos dados extraídos para o formato e estrutura desejada. Principais guias da transformação:
  - **Acurácia**: É a definição principal da etapa. A carga deve seguir com os dados brutos ou acurados? Qual a(s) informação(ões) deseja-se extrair da origem?
  - **Qualidade**: Quais as regras de negócio e de qualidade necessárias da transformação? Tipos e nomes dos dados, valores nulos, valores padrões, etc.
  - **Performance**: Como não onerar o processamento dos dados e seu consumo poterior? Pensar no antes e depois da geração dos dados.

* **Carga:**
Compreende a etapa de gravação dos dados transformados em um ou mais sistemas de destino. Pontos de atenção:
  - **Tipos de dados** (parquet e delta): A engenharia de dados é voltada para o big data, então a eficiência de armazenamento e leitura posterior é um ponto chave, como utilizar formatos parquet e delta, criados para dados massivos.

    **Mas nem sempre.**

    Excel, dashboard, etc. são destinos comuns dos dados. Então a necessidade do negócio é ponto determinante dessa etapa;
  - **Particionamento**: Palavra chave para performance e big data. É a divisão dos dados em partições, geralmente por data, para facilitar a leitura e processamento posterior;
  - **Compactação**: Aqui merece um tópico a parte, principalmente em cenários de ingestões streaming. É onde se materializa o problema fundamental de consulta x volume x custo dos dados;
  - **Tabelas temporárias**: Famosas _views_, recurso herdado do SQL, é o armazenamento em memória dos dados, sendo uma opção para processamento de dados com limitações de tamanho e tempo de vida. Use com moderação;
  - **Tabelas de histórico**: É a gravação dos dados em tabelas de histórico, geralmente com o objetivo de auditoria e rastreabilidade dos dados e gestão dos metadados.

### **2.1. ETLxELT:**
A transformação antes da gravação dos dados (ETL) tem entre outras vantagens o melhor aproveitamento da capacidade de armazenamento (menos dados vão para o destino) mas adicionam o tratamento dos dados na camada de processamento, tornando o processamento mais custoso.

A transformação após a gravação dos dados (ELT) permite principalmente a reutilização dos dados, mas pode trazer maior esforço de armazenamento e leitura dos dados com a necessidade de converter bits, strings para consumo dos dados.

O `depende` não é a resposta padrão atoa.

## **3. Camada Analítica:**

É o ambiente que armazena os dados dos times de dados. Mas se é possível consultar diretamente o MongoDB e SQL Server da vida, por que ter uma camada analítica?

### **3.1. Dados transacionais x Dados Analíticos:**
* O **processamento online de dados transacionais (Online Transaction Processing - OLTP)** é voltado para a performance e consistência dos dados, e gera os bancos de dados transacionais que o app e site em que estamos, utilizam para exibir essa tela e gravar esse texto por exemplo.
* O **processamento analítico (Online Analytical Processing - OLAP)** é voltado para a análise dos dados, onde velocidade da consulta e armazenamento massivo (olha o problema fundametal novamente) dos dados são os guias desse processamento. Ele gera dashboards, relatórios e os bancos de dados analíticos com toda essa ~~bagunça~~ informação.

### **3.2. Camadas analíticas:**
A camada tem camadas. Pensa em uma cebola. Agora imagina que colocaram uma senha para descascar a cebola, depois criaram um manual de como descascar a cebola.

Com essa analogia horrível, os principais modelos de camadas analíticas são:
* **Data Warehouse:** Aqui é a cebola com senha (Parei com as analogias). É o primeiro desenho de camada analítica, com a estruturação dos dados em tabelas dimensionais e fatos, tendo como fonte de extração os bancos transacionais, buscando facilitar a consulta dos dados mas mantendo diretriizes de qualidade e consistência dos dados como dos dbs transacionais.
* **Data Lake:** É o filho millenial do Warehouse, plugado na internet e que descobriu o json e o csv. Passa a olhar para os dados além do transacional, sem se preocupar com a estrutura das informações que mantém. Foi o primeiro passo para o Big Data.
* **Data Lakehouse:** É o filho do lake, que voltou a morar com os avós (não consegui fugir dessa). É a junção do Data Lake com o Data Warehouse, com a estruturação dos dados em tabelas dimensionais e fatos, mas com a capacidade de armazenamento massivo do Data Lake.

  Duas palavras chave do Lakehouse: Metadados e Governança. Voltamos a elas em breve.
* **Data Mesh:** Não é bem uma camada, sonha em ser uma estratégia empresarial, mas funciona com uma arquitetura de tecnologia no mundo do lakehouse. É a descentralização da camada analítica, trazendo ela para as frentes do negócio, para engenharia de software e onde mais puder chegar. A idéia é que se você cria seus dados, você cuida deles.
* **Data Swamp:** Ninguém quer assumir o pântano que virou se lake mas acontece. É o Data Lake sem governança, sem qualidade, sem estrutura. Geralmente grande demais para resetar e necessário para ser abandonado. Ningúem fala dos fracassos, mas eles existem.

### **3.3. Arquitetura das camadas analíticas:**
Lembra do T do Processamento? É aqui que se vê o trabalho dele. A arquitetura das camadas analíticas é o que vai definir o processamento dos dados, a estruturação dos dados e a consulta dos dados.
- **Bronze**: É a camada de dados brutos, sem tratamento, sem transformação, sem nada. É a camada de dados transacionais, mas também pode ser a camada de dados de origem de um data lake.
- **Prata**: É a camada de dados tratados, mas sem estruturação. É a camada de dados de um data lake, mas também pode ser a camada de dados de origem de um data warehouse.
- **Ouro**: É a camada de dados tratados e estruturados. É a camada de dados de um data warehouse, mas também pode ser a camada de dados de origem de um data lakehouse.

## **4. Principais produtos:**
Seja criando ou fazendo o meio de campo com produtos de mercado, atuando em plataforma, dataops ou como enablers, alguns produtos de dados comuns em várias empresas são:
- **Catálogo de dados**: Lembra dos metadados do lakehouse? É aqui que eles valem ouro. A ideia do catálogo é guiar o consumos dos dados das camadas analíticas à partir das informações dos artefatos de dados que ela tem.
  Um subproduto do catálogo é a linhagem de dados. É a árvore genealógica do artefato. Tabelas, arquivos e o que mais originou o artefato, e o que o artefato originou.
  - Ps: **Afinal o que são metadados?** São os dados dos dados. É a informação sobre a informação. É o nome da tabela, o nome da coluna, o tipo de dado, o tamanho do dado, etc.

    Em arquivos parquet e delta, citados antes, a criação, atualização e armaenamento dos metadados é automática por exemplo.

    Metadados está para a engenharia de dados assim como os dados também estão (Não sei se fez sentido mas é pra reforçar a importância deles).
- **Camada semântica**: De novo do lakehouse. É uma camada preocupada em dar sentido, padronização e aplicar regras de negócio aos dados. É a camada de dados tratados e estruturados, mas com a visão do negócio.

  Pode chamar de governança de dados também e peça chave para estabelecer uma **fonte única da verdade (single source of true - SSOT)**
- **Data Quality**: Qualidade de maneira geral é a tolerância a falhas de um produto ou serviço. Em dados é estabelecer a mesma tolerância para criação, atualização e consumo dos dados à partir das expectativas com ele (% de valores nulos, de campos com determinado nome, etc.). Independente da empresa, um produto de data quality se não existe vai existir.
- **Pipelines de dados** (ADF, Databricks Jobs): É a automação do ETL/ELT. Não é difícil imaginar a importância disso. Geralmente utiliza-se ferramentas de orquestração ou de terceiros para isso.
- **Orquestradores** (Airflow, dbt, Pentaho): São o Lego da engenharia de dados. São ferramentas que permitem a criação de pipelines de dados, mas também de processos de dados, de forma automatizada e com a possibilidade de reutilização de código.
- **Interfaces de Cloud** (Databricks, Cloudera, Snowflake): São as interfaces de cloud que permitem a criação de ambientes de dados, com a possibilidade de criação de pipelines de dados, processos de dados, etc.

## **5. Principais ferramentas/habilidades:**
Por último e mais importante, as ferramentas e habilidades necessárias para a engenharia de dados da China ao Brasil são:
- **Business Skills**: Contra dados só há o orçamento. Das especialidades de dados, engenharia é a mais distante do cliente final da empresa. Seus clientes são internos (os outros times de dados) e o impacto no faturamento da empresa é indireto.
  Justamente por ter esse distanciamento, entender o negócio e o impacto do seu trabalho nele permite melhor interação com os clientes, e alinhamento de expectativas e prioridades dos produtos entregues.
- **Modelo dimensional**: Com todo respeito a arquitetura mas engenharia também desenha. Aqui em específico é o desenho dos bancos analíticos, que diferente dos transacionais, são desenhados para consulta e não para gravação. Vale uma pesquisa com carinho a respeito.
- **Python**: Se distante do cliente final, a engenharia de dados é próxima do desenvolvimento de software. O python vem tomando o lugar do Javascript como linguagem de programação mais utilizada, mas para os profissionais de dados já é a primeira, as vezes única, opção há muito tempo.
Não só para consulta mas na construção de soluções de dados, o python tem que estar na caixa de ferramentas do engenheiro de dados.
- **Spark**: É a principal solução de processamento de big data. Todos os produtos, ferramentas e até o python e outras linguagens, tem integração com o spark. Não passe batido por ele.
- **SQL, Java, R e Scala**: As principais linguagens de consulta de dados e o Java. O SQL é a linguagem de consulta de dados mais utilizada, mas o Scala e o R tem suas aplicações em processamento de dados e análise de dados respectivamente. Java é a linguagem mãe da programação moderna. Só isso e tudo isso.

  Depois do python, a proficiência em SQL é a habilidade mais comum em engenharia de dados. Mais uma dessas linguagens não são mandatórias mas agregam muito.
- **Cloud**: Habilidades de cloud invariavelmente passam pelo caminho da engenharia de dados. Seja para criação de ambientes, pipelines de dados, processos de dados, etc. A cloud é o caminho para a engenharia de dados ligada a big data.
- **DBA/ DevOps/Full Stack Software Engineer Skills**: Engenharia de dados é uma das especialidades de dados mais recentes. É comum que assuma funções de análise de dados e outros segmentos na área em algumas atribuições de vagas justamente por estar em construção seu papel em algumas organizações. 
  A certeza, porém, é que ela acumule funções e requisitos de DBAs, DevOps e Full Stack Software Engineers. Não é obrigatório, mas é comum mais essas sopas de letrinhas como habilidades da profissão:
  - **SGBD**
  - **NoSQL**
  - **Mensageria**
  - **Backend Software Engineer (APIs)**
