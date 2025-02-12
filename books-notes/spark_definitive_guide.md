Esse é um resumo do livro Spark: The Definitive Guide, de Bill Chambers e Matei Zaharia, que cobre o Apache Spark 2.0.

Os tópicos destacados são os que cobrem  arquitetura e functionamento por debaixo do capô do Spark, apesar do livro seguir válido como referência para as releases atuais do Spark quanto a seus outros temas.

As referências de número de página seguem a versão [desse repositório](https://github.com/VolodymyrGavrysh/My_RoadMap_Data_Science/blob/master/kds/books/Spark-The%20Definitive%20Guide.pdf)

<h1><a id="topicos">Tópicos:</a></h1>

- 1. [O que é Spark](#o-que-e-spark)
  - 1.1. [O problema da big data](#big-data)
  - 1.2. [História do Spark](#history)
- 2. [Uma introdução gentil ao Spark](#spark-intro)
  - 2.1. [Arquitetura básica do Spark](#spark-architecture)
  - 2.2. [Spark languages API](#spark-languages)
  - 2.3. [SparkSession](#spark-session)
  - 2.4. [DataFrames](#dataframes)
    - 2.4.1. [Partições](#particoes)
  - 2.5. [Transformations](#transformations)
  - 2.6. [Actions](#actions)
  - 2.7. [Spark UI](#spark-ui)
- 3. [Overview de Structured Spark Types](#structured-spark-types)
  - 3.1. [Overview da execução de uma API Estruturada](#structured-api-execution)
    - 3.1.1. [Logical Planning](#logical-planning)
    - 3.1.2. [Physical Planning](#physical-planning)
    - 3.1.3. [Colunas](#columns)
    - 3.1.4. [Registros e linhas](#rows)
- 4. [Operações Estruturadas básicas](#basic-structured-operations)
  - 4.1. [DataFrame transformations](#dataframe-transformations)
    - 4.1.1. [Repartition e Coalesce](#repartition-coalesce)
- 5. [Agregações](#aggregations)
- 6. [Joins](#joins)
- 7. [Spark SQL](#spark-sql)
- 8. [Low level APIs](#low-level-apis)
- 9. [Spark Applications](#spark-applications)
  - 9.1. [Execution modes](#execution-modes)
  - 9.2. [Life cycle da Spark Application (fora do Spark)](#life-cycle-outside-spark)
  - 9.3. [Life cycle da Spark Application (no Spark)](#life-cycle-inside-spark)
  - 9.4. [Detalhes da execução](#execution-details)
  - 9.5. [Spark envs](#spark-envs)
  - 9.6. [Spark deploys](#spark-deploys)
- 10. [Monitoring](#monitoring)
- 11. [Performance tuning](#performance-tuning)

<h2><a id="o-que-e-spark">1. O que é Spark:</a></h2>

  ##### [Voltar ao topo](#topicos)
  Um motor de computação unificado e um conjunto de bibliotecas para processamento paralelo de dados em clusters de computadores.

  Suporta uma variedade de linguagens de programação (Python, Java, Scala e R), incluindo bibliotecas para tarefas diversas cobrindo do SQL ao streaming e machine learning, rodando de um laptop à um cluster de milhares de servidores. 

<h3><a id="big-data">1.1. O problema da big data:</a></h3>

  ##### [Voltar ao topo](#topicos)
  Como muitas das tendência em computação, mudanças em fatores econômicos acarretam mudanças em aplicações e hardwares computacionais. 

  Os computadores se tornaram mais rápidos com o aumento na velocidade de seu processamento, permitindo a execução de mais instruções por segundo a cada ano. 
  Como resultado, sem mudanças no código aplicações se tornavam mais rápidas. Essa tendência levou a um grande e estabelecido ecossistema construindo ao longo dos anos de aplicações desenhados para serem eecutadas em um único processador. Essas aplicações encabeçaram a tendência de melhorar a velocidade de processamento para escalar maior computação e volumes de dados ao longo do tempo. 

  Essa tendência dos hardwares parou em torno de 2005 com limites difíceis na dissipação de calor fazendo os desenvolvedores de hardware pararem a construção de processadores individuais mais rápidos, trocando para a adição de mais cores de CPU paralelos rodando na mesma velocidade.  
  A consequência foi a imediata necessidade das aplicações serem modificadas para adicionar paralelismo para serem executadas mais rápidas, trazendo ao estágio de modelos de programação como o Apache Spark. 

  Além disso, as tecnologias de armazenamento e coleta de dados não diminuíram seu ritmo em 2005 como a velocidade de processamento fez. O custo de armazenamento de 1 TB de dados continuou a cair em torno de 2 vezes a cada 14 meses, significando que é muito barato para organizações de todos os tamanhos armazenarem alta quantidade de dados. Ainda, muitas das tecnologias de coleta de dados (sensores, cameras, conjuntos de dados públicos, etc..) continuaram a diminuir seus custos e melhoraram em qualidade. 

  O resultado final é um mundo em que coletar dados é extremamente barato, muitas empresas consideram negligência não registrar dados possivelmente relevantes para o negócio, mas o processamento requer maior computação paralelizada frequentemente em clusters de máquinas.  
  Ainda, nesse novo mundo, o desenvolvimento de software dos últimos 50 anos não é automaticamente escalável, e nem os modelos de programação tradicionais podem criar os novos modelos de programação necessário.

  O Apache Spark é construído para esse novo mundo (palavras dos criadores). 

<h3><a id="history">1.2. História do Spark:</a></h3>

  ##### [Voltar ao topo](#topicos)
  O Spark surge em pesquisas na UC Berkeley em 2009 como projeto de pesquisa, buscando evoluir o Hadoop MapReduce, o principal motor de programação paralela para clusters da época. Sua primeira publicação foi o paper “Spark: Cluster Computing with Working Sets” by 
  Matei Zaharia, Mosharaf Chowdhury, Michael Franklin, Scott Shenker, and Ion Stoica of the UC Berkeley AMPlab. 

  Dois principais achados iniciais foram: i. o grande potencial da computação por clusters, com as empresas que usavam o MapReduce enxergando várias aplicações possíveis usando os novos dados e vários grupos utilizando o sistema já com seus casos de uso iniciais; ii. o desafio e ineficiência do MapReduce em construir grandes aplicações. 
  Um exemplo é um job de machine learning que 10 a 20 vezes por um dataset, onde no MapReduce cada execução precisaria ser escrita como um job que será executado separadamente no cluster carregando os dados do zero.

  Para esse problema, o time do Spark desenhou uma API baseada em programação funcional, que pode brevemente declarar aplicação com várias etapas, executando em memória (mais eficiente) e compartilhando os dados entre as várias etapas de cálculo. 

<h2><a id="spark-intro">2. Uma introdução gentil ao Spark:</a></h2>
<h3><a id="spark-architecture">2.1. Arquitetura básica do Spark:</a></h3>
  
  ##### [Voltar ao topo](#topicos)
  O processamento de dados é uma área desafiadora por envolver a necessidade de poder de processamento e orquestração dos seus fluxos. 

  Um cluster, como comentado, agrupa o poder de várias máquinas juntas como se fossem um único computador. Uma estrutura que coordene o trabalho ao longo do cluster é o que o Spark faz, gerenciando e coordenando a execução de tarefas dos dados ao longo do cluster de computadores. 

  O cluster de máquinas usado pelo Spark é gerenciado por um cluster manager, como Spark’s standalone cluster manager, YARN, Mesos e o local mode. A aplicação Spark é submetida ao cluster manager que garantirá os recursos para a aplicação para completar o trabalho. 

  Essa é a arquitetura física do Spark. 

  * Aplicação Spark (Spark Applications): 
  A Spark Application consiste em um processo do driver e um conjunto de processos dos executors.

  * **O processo do driver** executa a função main(), que fica em um nó do cluster, e é responsável por três coisas: 
  1. Manter informações sobre a Spark Application; 
  2. Responder ao programa do usuário ou entrada 
  (input); 
  3. Analisar, distribuir e agendar o trabalho (work) ao longo dos executors.

  O processo do driver é o coração da Spark Application e mantém todas as informações relevantes durante o tempo de vida da aplicação. 

  * **Os executors**: são responsáveis por de fato realizar o work que o driver atribui a eles. 
  Isso significa que cada executor é responsável por somente duas coisas: 
  1. Executar o código atribuído a ele pelo driver; 
  2. Reportar o estado do seu processamento de volta ao nó do driver. 

  * Cluster manager:
    |— Driver Process 
      |— Código do usuario 
      |— Spark Session 
    |———|— Executors 

    * O Cluster manager troca informação com driver e os executors
    * O driver através da Spark Session dispara os Executors 

    > NOTA: Com o local mode como cluster manager, o driver e executor (que são por detrás dos panos apenas processos), são executados como threads em uma máquina local ao invés de um cluster. 

  * Pontos chave: 
    Spark emprega um gerenciador de cluster que monitora os recursos disponíveis 
    O processo do driver é responsável por executar os comandos do programa do driver através dos executores para completar uma determinada tarefa. 

<h3><a id="spark-languages">2.2. Spark languages API:</a></h3>

  ##### [Voltar ao topo](#topicos)
  Os executors na maior parte do tempo sempre executará código Spark, o driver porém, pode ser disparado por um número diferentes de linguagens através das APIs de linguagem do Spark. O Spark tem alguns “conceitos” chave em cada linguagem (um objeto SparkSession para cada linguagem), esses conceitos são traduzidos em Spark code que executa no cluster de máquinas. 
  Utilizando as Structured APIs, é esperado que todas as linguagens tenham características parecidas de performance. 
  As linguagens com suporte oficial do Spark: _Scala (linguagem em que o Spark é criado, se tornando a linguagem padrão) 
  * Java 
  * Python 
  * SQL (um subconjunto do padrão ANSI SQL 2003) 
  * R 

<h3><a id="spark-session">2.3. SparkSession:</a></h3>

  ##### [Voltar ao topo](#topicos)
  É um processo do driver pelo qual o Spark executa as manipulações definidas pelos usuários ao longo do cluster.

  Há uma SparkSession para uma Spark Application, sendo a aplicação controlada pela SparkSession. 

<h3><a id="dataframes">2.4. DataFrames:</a></h3>

  ##### [Voltar ao topo](#topicos)
  A mais popular Structured API, que representa uma tabela de dados com linhas e colunas. A lista que define as colunas e os tipos dessas colunas é chamada de **schema**.

  Uma abstração é pensar nele como uma planilha com colunas nomeadas.

  Importante que os dados do DataFrame pode ser distribuído em mais de uma máquina do cluster, seja por seus dados ou o processamento ser oneroso para apenas uma máquina. 

  DataFrame é um conceito existente no Python e R também, porém geralmente eles existem em uma única máquina ao contrário do Spark. Também através das APIS com Python e R, é fácil converter os dataframes dessas linguagens para um dataframe Spark. 

  > NOTA: Há outras coleções do Spark similares aos dataframes como os Datasets, SQL tables e Resilient Distributed Datasets (RDDs), mas os DataFrames são os mais eficientes e fáceis de manipular, e foco das certificações e novas releases do Spark. 

<h3><a id="particoes">2.4.1. Partições:</a></h3>

  ##### [Voltar ao topo](#topicos)
  Para permitir que os executors trabalhem em paralelo, o Spark quebra os dados em pedaços chamados de partições.

  Uma partição é uma coleção de linhas situadas em uma máquina física em um cluster.  

  Uma partição do DataFrame representa como o dado é fisicamente distribuído ao redor do cluster de máquinas durante sua execução. 

  Com uma partição o Spark executa um paralelismo de somente um, mesmo que tenha disponível milhares de executors. Se houver várias partições mas somente um executor, O Spark também terá o paralelismo de um porque só há um recurso computacional. Importante que através dos DataFrames não é necessário manipular as partições manual ou individualmente na maioria dos casos. Com a pessoa focando em transformações de alto nível e o Spark determinando o funcionamento da execução no cluster. 

<h3><a id="transformations">2.5. Transformations:</a></h3>

  ##### [Voltar ao topo](#topicos)
  São a chave para expressar sua lógica de negócio usando Spark. 

  * As transformações são **narrow**, quando cada partição de entrada irá contribuir para uma partição de saída. 
  * As transformações **wide** tem partições de entrada contribuindo para muitas partições de saída. 

  A grande diferença entre elas é que as transformações narrow conseguem ser executadas em memória. Nas wide acontece o embaralhamento (shuffle), com o Spark usando o disco dos executors na escrita dos dados. 

<h3><a id="actions">2.6. Actions:</a></h3>

  ##### [Voltar ao topo](#topicos)
  Dispara o processamento do plano lógico de transformação. Três tipos de actions disponíveis: 
  * para visualizar dados no console 
  * para coletar dados para objetos nativos da respectiva linguagem (.collect()) 
  * para escrever em dados de saída 

  Ao especificar uma ação, um Spark Job é iniciado executando tipicamente: as transformações narrow, em seguida as wide, e a ação que pode ser de um dos tipos comentados acima 

<h3><a id="spark-ui">2.7. Spark UI:</a></h3>

  ##### [Voltar ao topo](#topicos)
  Uma ferramenta inclusa no Spark que permite o monitoramento dos jobs spark executando em um cluster. 

  Ela é disponível na porta 4040 do nó do driver. 

  Localmente é: <http://localhost:4040/>

  Nela é demonstrado informações sobre o estado dos Spark Jobs, seu ambiente e estado do cluster 

  * Spark Job: representa um conjunto de transformações disparadas por uma action, que pode ser monitorado da Spark UI. 


  * **Sumarizando até aqui**: 
    * Spark é um modelo de programação distribuído no qual o usuário especifica transformações.  
    * Múltiplas transformações constroem uma DAG (gráfico acíclico distribuído) de instruções. 
    * Uma ação (action) inicia o processo de execução do grafo de instruções, como um único job, o quebrando em stages e tasks a serem executadas ao logo de um cluster 
    * As estruturas lógicas para manipuladas com transformações e ações são DataFrames e Datasets; - Para criar um DataFrame ou Dataset é chamada uma transformação; 
    * Para iniciar o processamento ou converter para tipos nativos das linguagens uma action é chamada 

<h2><a id="structured-spark-types">3. Overview de Structured Spark Types (Capítulo 4 do livro):</a></h2>

  ##### [Voltar ao topo](#topicos)
  Spark é efetivamente uma linguagem de programação. Internamente, ele usa um motor chamado de Catalyst que mantem seu próprio tipo de informações através do plano e processamento de trabalhos. 

  Com isso é aberta uma variedade de otimizações de execução que faz diferença significativa. 

  Para o Spark (Em Scala), DataFrames são Datasets do tipo Row (p. 57), uma representação interna do seu formato in-memory otimizado para processamento. Esse formato feito para computação altamente especializada e eficiente porque não usa os tipos da JVM que pode causar alto garbage collection e custos de instanciação dos objetos, com o Spark operando no seu próprio formato interno sem incorrer em qualquer um desses ofensores. 

  Nota (minha): Os tipos do Spark tem uma equivalência direta dos tipos em Scala e Java, como BinaryType que é um Array[Byte] do Scala, DateType que é java.sql.Date, e ArrayType que é Scala.collection.Seq. 

  Uma referência geral está na página 60 desse livro. 

<h3><a id="structured-api-execution">3.1. Overview da execução de uma API Estruturada:</a></h3>

  ##### [Voltar ao topo](#topicos)
  1. Escrita do código do DataFrame/Dataset/SQL; 
  2. Se o código é válido, Spark o converte em um plano lógico (logical plan) 
  3. O Spark transforma o Logical Plan em um Physical Plan, verificando optimizações no caminho; 
  4. Spark executa o Physical Plan (RDD manipulations) no cluster. 

  Independente da linguagem que o código foi criado (Python, Scala, …) ele é passado para o Catalyst Optimizer que decide como o código deve ser executado e constrói um plano dessa execução, e finalmente o código é executado e o resultado retornado para o usuário. 

<h3><a id="logical-planning">3.1.1. Logical Planning:</a></h3>

  ##### [Voltar ao topo](#topicos)
  A primeira fase da execução é converter o código do usuário em um plano lógico. 

  Código do usuário 
  |—Unresolved logical plan 
      |—Catalog—Analysis 
              |—Resolved logical plan 
                |—Logical Optimization 
                  |— Optimized logical plan 

  Esse plano representa um conjunto de transformações abstratas convertendo o código do usuário na versão mais otimizada possível. Ë a primeira etapa chamada de unresolved logical plan, porque apesar do código ser válido, as tabelas e colunas especificadas nele podem não existir. 

  Em seguida o Spark usa o catálogo, um repositório com todas as informações de tabelas e DataFrames para resolver as colunas em tabelas no analyzer. O analizer pode rejeitar o unresolved logical plan se os atributos não forem achados no catálogo. Se forem resolvidos, o resultado passa para o Catalyst Optimizer, uma coleção de regras que tentam otimizar o plano lógico reajustando predicados e seleções. Pacotes podem extender o Catalyst para incluir suas próprias regras para otimizações específicas de domínios.  

<h3><a id="physical-planning">3.1.2. Physical Planning:</a></h3>

  ##### [Voltar ao topo](#topicos)
  Criando um optimized logical plan, O Spark inicia o processo de criação de um physical plan, ou Spark plan, especificando como o logical plan será executado no cluster, em diferentes estratégias de execução física comparadas em um modelo de custos (cost model) 

  Optimized logical plan 
    |—Physical Plan 1 
  |—Physical Plan 2 
  |—Physical Plan n 
      |—Cost Model 
        |—Best Physical Plan 
          |— Execução no cluster 

  Plano físicos resultam numa séries de RDDs e transformações, o que pode ser considerado um compilador do Spark, que recebe consultas em dataframes, datasets e SQL e as compila em transformações RDD. 
    
  Ao selecionar um plano físico, Spark executar o código sobre RDDs, a camada mais baixa de interface de programação do Spark. Ainda, em tempo de execução (runtime) é gerado Java bytecode que pode remover tasks e stages completamente durante a execução. 

  Por fim os resultados são retornados aos usuários. 

<h3><a id="columns">3.1.3. Colunas (p. 60):</a></h3>

  ##### [Voltar ao topo](#topicos)
  Para o Spark, colunas são construções lógicas que representam um valor computado em um registro por meio de uma expressão. Isso significa que para ter um valor real para uma coluna, é necessário ter uma linha, e para ter uma linha é necessário um DataFrame. Ou seja, não é possível manipular uma coluna fora do contexto de um DataFrame. 

  * Aprofundando: 
    * Colunas são expressões; 
    * Colunas e transformações dessas colunas compilam no mesmo plano lógico que expressões analisadas. expr(“someCol” -5) == col(“someCol”) -5

<h3><a id="rows">3.1.4. Registros e linhas (p. 70):</a></h3>

  ##### [Voltar ao topo](#topicos)
  No Spark, cada linha do DataFrame é um único registro. Spark representa esse registro com um objeto do tipo Row. 

  O Spark manipula esses objetos usando expressões de colunas buscando produzir valores usáveis. 

  Internamente eles são arrays de bytes mas que nunca são mostrados aos usuários por serem manipulados com expressões de colunas 

<h2><a id="basic-structured-operations">4. Operações Estruturadas básicas (Capítulo 5 do livro):</a></h2>

  ##### [Voltar ao topo](#topicos)

<h3><a id="dataframe-transformations">4.1. DataFrame transformations:</a></h3>

  ##### [Voltar ao topo](#topicos)
  As transformações podem ser sumarizadas em quatro operações:

  * Adicionar linhas ou colunas 
  * Remover linhas ou colunas 
  * Transformar uma linha em coluna (ou vice-versa) - Mudar a ordem das linhas baseado no valores nas colunas 

  >NOTA:  Por padrão Spark é case insensitive, o que pode ser alterar com a conf: spark.sql.caseSensitive true 

<h3><a id="repartition-coalesce">4.1.1. Repartition e Coalesce (p. 86):</a></h3>

  ##### [Voltar ao topo](#topicos)
  Buscando particionar os dados de acordo com a frequência de colunas filtradas, podendo controlar o desenhou físico dos dados ao redor do cluster, seja com o particionamento do schema e o número de partições é através dos métodos repartition e Coalesce. 

  O primeiro faz um embaralhamento total dos dados, significando que deve ser usado quando o número de partições futuras é maior do que o número atual ou quando se busca particionar por um conjunto de colunas. 

  Quando uma certa coluna é frequentemente usada como filtro, é uma candidata a ser particionada: df.repartition(col(“country”) 

  Também sendo possível especificar o número de partições em conjunto: 
  ```python
  df.repartition(5, col(“country”) 
  ```

  **Coalesce** não fará um shuffle completo, e tentará combinar as partições. 
  ```python
  df.repartition(5, col(“country”).coalesce(2) 
  ```

  _Colletando linhas para o Driver: 
  Cuidado porque para grandes conjuntos de dados pode facilmente crasear o nó do driver que irá receber esses dados. 

  > NOTA: é possível determinar a timezone do job com a conf abaixo: 
  ```python
  spark.conf.sessionLocalTimeZone(<javaTimeZoneFormat>) 
  ```

<h2><a id="aggregations">5. Agregações (Capítulo 7 do livro):</a></h2>

  ##### [Voltar ao topo](#topicos)
  Podem ser: 
  * a nível do dataframe inteiro (sum, max, …) 
  * agrupamento (groupBy) 
  * window functions 
  * grouping sets: uma ferramenta de baixo nível para combinar conjuntos de agregação. Disponível somente no SQL 
  * rollup: solução para os dataframes semelhante ao grouping sets, é uma agregação multidimensional que executa uma variedade de cálculos no estilo groupby _cube: semelhante ao rollup, também é uma solução de grouping sets, mas ao invés de tratar os dados hierarquicamente, um cubo faz a mesma coisa entre todas as dimensões. 
  * pivot: possibilita converter uma linha em coluna 

<h2><a id="joins">6. Joins (Capítulo 8 do livro):</a></h2>

  ##### [Voltar ao topo](#topicos)
  * Crossjoin: pode ser necessário habilitar ele para evitar warnings ou que o spark tente fazer outro join ao invés do cross (p. 153): spark.sql.crossJoin.enable true 
  
  * Join entre big e small table: 
  No atributo other do join, use a função broadcast no dataframe pequeno.  
  * Importante: um broadcast em uma tabela grande pode quebrar o nó do driver. 

<h2><a id="spark-sql">7. Spark SQL (Capítulo 10 do livro):</a></h2>

  ##### [Voltar ao topo](#topicos)
  * Writing data em paralelo: 
  O número de arquivos escritos depende do número de partições do DataFrame na hora da escrita. Um repartition/coalesce pode ser usado para controlar a quantidade de arquivos resultantes. 
  
  * Bucketing: 
  Semelhante ao partitionBy, o bucket agrupa permite especificar o dado que é escrito em cada arquivo pelas características das colunas informadas no bucketBy 
  
  * Gerindo o tamanho dos arquivos: 
  A opção de escrita `“masRecordsPerFile”` permite especificar o número de registros por arquivo dando um nível de controle do tamanho desses arquivos evitando problemas de small files por exemplo. 
  
  > NOTA: Um count(*) no Spark considera os valores nulos, porém contando uma coluna individual Spark não contará os valores nulos. 
  
  * CONFS PARA TABELAS SPARK:
  ```python
  spark.sql.files.maxPartitionBytes 
  spark.sql.shuffle.partitions
  ```

<h2><a id="low-level-apis">8. Low level APIs (Capítulo 12 do livro):</a></h2>

  ##### [Voltar ao topo](#topicos)

  A SparkContext, que é acessível nas SparkSession, são os entrypoints para as funcionalidades das APIs de baixo nível.

  _Casos de uso:´ 
  > Uma funcionalidade inexistente nas API de alto nível, como maior controle do plano físico a ser executado > Manutenção de códigos legados escritos em RDDs > Necessidade de manipulação em alguma variável customizada compartilhada (que serão usadas nas 
  UDFs)

  **RDD**: Na prática, o RDD tem muitas das funções das APIs estruturadas, para algumas a diferença é que a manipulação ocorre na linguagem do usuário, parecido com as UDFs. 

<h2><a id="spark-applications">9. Spark Applications (Capítulos 15 a 17 do livro):</a></h2>

  ##### [Voltar ao topo](#topicos)

<h3><a id="execution-modes">9.1. Execution modes (p. 255):</a></h3>

  ##### [Voltar ao topo](#topicos)
  * **Cluster mode**: O jeito mais comum de executar Spark Applications, com a pessoa submetendo seu código para o cluster manager, que lança o processo do driver em um worker node dentro do cluster em conjunto ao processamento do executor. Em suma, o cluster manager é responsável por manter todos os processos relacionados com a Spark Application. 
  * **Client mode**: Parecido com o cluster mode, com exceção que o Spark Driver permanece na máquina do cliente que submete a aplicação. Assim, a máquina do cliente é responsável por manter o processo do Spark Driver, e o cluster mantem o processo dos executors.
  Basicamente, a máquina que executa o driver é externa ao cluster, também conhecidas como gateway machines ou edge nodes.
  * **Local mode**: Diferente dos outros modos, ele executa a Spark Application em uma única máquina, atingindo o paralelismo através das threads dessa única máquina.
  Comum para aprender sobre Spark e testar suas aplicações, não é recomendado para aplicações em produção.

<h3><a id="life-cycle-outside-spark">9.2. Life cycle da Spark Application (fora do Spark):</a></h3>

  ##### [Voltar ao topo](#topicos)
  Pensando na infraestrutura que mantém o Spark, esse é o ciclo de vida da Spark Application:

  1. Client request: A pessoa submete seu código ao driver node do cluster manager que aloca o driver em um nó do cluster.

  2. Launch: Com o driver alocado no cluster, é iniciada a execução do código do usuário que inclui uma SparkSession que inicia um Spark Cluster (driver+executors). 
  A SparkSession na sequência se comunica com o cluster manager requerendo que o processo do Spark Executor seja lançado no cluster.
  O número de executors e as configurações relevantes podem ser configuradas pelo usuário na submissão do código.

  O cluster manager responde alocando o processos dos executors e enviando a informação relevante das locations para o processo do driver.
  Com tudo engatilhado corretamente, têm-se o Spark Cluster.

  3. Execução: Com o Spark Cluster, o driver e Workers se comunicam entre si executando o código e movendo os dados ao longo deles.
  O driver agenda tasks em cada worker, e cada worker responde com o status dessas tasks de sucesso ou falha.

  4. Com a Spark Application completada, o processo do drive finaliza com sucesso ou falha. O Cluster manager então interrompe os executors no spark cluster para o driver.
  Nesse ponto é possível consultar o cluster manager sobre o status da execução da Spark Application.

<h3><a id="life-cycle-inside-spark">9.3. Life cycle da Spark Application (no Spark):</a></h3>

  ##### [Voltar ao topo](#topicos)
  Dentro do Spark, esse é o ciclo de vida da Spark Application, basicamente o código do usuário.

  Cada aplicação é construída por um ou mais Spark Jobs, que são executados de forma serial a menos que seja usada threading para ativar múltiplas ações em paralelo.

  1. SparkSession: A primeira ação de qualquer Spark Application é criar uma SparkSession. Em muitos dos modos interativos, isso é feito por você, mas é uma aplicação, isso deve ser feito manualmente.

  Depois de ter uma SparkSession, deve ser habilitada a execução do código Spark.

  Da SparkSession é possível acessar todos os contextos e configurações legados e de baixo nível.

  2. SparkContext: objeto dentro da SparkSession que representa a conexão com o Spark cluster.

  É através dela que é habilitada a comunicação com algumas API de baixo nível do Spark como RDDs.

  Através da SparkContext é possível criar RDDs, acumulators, broadcast variables e rodar códigos no cluster.

  3. Instruções lógicas para execução física: A conversão do código em quaisquer das linguagens suportadas é enviada para  um plano de execução através de uma action, que se desdobra nas seguintes estruturas:

  * Spark job: Em geral deve haver um Spark job para cada action. Actions retornarão um resultado. Cada job é repartido em uma série de stages, com o número deles dependendo em quantas operações de shuffle precisam ocorrer.
  * Stages: São grupos de tasks que podem ser executadas juntas para computar a mesma operação em múltiplas máquinas. Em geral o Spark tentará empacotar mais trabalhos juntos quanto possível (isso é, quantas transformações quanto possível em um job) no mesmo stage, mas o motor inicia novos stages depois das operações de shuffle.

  Um shuffle representa uma repartição física do dado, como um agrupamento ou ordenamento do dado processado. O time de reparticionamento exige coordenação entre os executors para mover os dados entre eles.

  O Spark inicia um novo stage depois de cada shuffle, e acompanha a ordem de execução que os stages devem seguir para computar o resultado final.

  Uma regra de bolso é que o número de partições deve ser maior que o número de executors do cluster, potencializado por muitos fatores dependendo da carga de trabalho. 

  Contudo, executando na máquina local, é importante manter o valor baixo pois a máquina local pode não compartimentar a quantidade de Jobs em paralelo.

  * Tasks: Stages no spark consistem em tasks, que correspondem a uma combinação de blocos de dados e um conjunto de transformações que serão executadas em um único executor.

  Se houver uma partição do dado, será executada uma task. Se houver 1000 partições, resultarão em mil tasks em paralelo.

  Ela é uma unidade computacional aplicada a uma unidade de dado (a partição).

  Particionar seus dados em um grande número de partições significa que mais serão executadas em paralelo. 

  Não é um almoço grátis, mas é um jeito simples para iniciar com a otimização.

<h3><a id="execution-details">9.4. Detalhes da execução:</a></h3>

  ##### [Voltar ao topo](#topicos)
  O Spark automatiza pipelines de stages e tasks que podem ser executadas juntas, como uma operação de map seguida por outra. Também para todo shuffle, o Spark escreve os dados em um storage estável ( o disco) podendo reutilizar esse dado em múltiplos Jobs.
  * Pipelining: Uma parte importante do que faz o Spark ser uma ferramenta computacional “in memory” é que ele busca realizar quantos passos seja possível de vez antes de escrever os dados da memória para o disco.

  Uma das chaves dessa otimização são as pipelines que ocorrem no nível do RDD.

  Com o pipelinig, uma sequencia de operações que alimentam os dados diretamente em cada uma, sem precisar movê-los entre os nós, é agrupada em um único stage de tasks que faz todas a operações em conjunto.

  * Shuffle Persistence: Em uma operação que precisa mover os dados ao longo dos nós, o motor não consegue executar o pipelining e no seu lugar executa um shuffle.

  Salvar os dados em disco do shuffle permite o Spark gerenciar falhas ou intermitências quando não há executors disponíveis para processá-los, permitindo um arranjo de reduzir a reexecução de tasks com falhas, preservando os dados que seriam processados nelas.

<h3><a id="spark-envs">9.5. Spark envs (p. 282):</a></h3>

  ##### [Voltar ao topo](#topicos)

  * **Job Scheduling**: tipicamente o scheduler segue um FIFO., que pode ser personalizado pela conf spark.scheduler.mode (mais na página 283)

<h3><a id="spark-deploys">9.6. Spark deploys (p. 285 a 295):</a></h3>

  ##### [Voltar ao topo](#topicos)

<h2><a id="monitoring">10. Monitoring (Capítulo 18 do livro, p. 296 a 316):</a></h2>

  ##### [Voltar ao topo](#topicos)

  * out of memory error:
  Soluções comuns: 
  * aumento da memória disponível e número de executors
  * Aumento do tamanho dos Workers via configurações pythob
  * Investigar mensagens de erro do garbage collector
  * Tratamento correto de dados nulos, para casos de “ “ e “EMPTY”
  * Evitar UDFs

<h2><a id="performance-tuning">11. Performance tuning (Capítulo 19 do livro, p. 317 a 330):</a></h2>

  ##### [Voltar ao topo](#topicos)
