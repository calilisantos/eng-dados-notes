## **Workspaces Databricks:**

* ### **Data Plane:**
  Onde o dado é processado na plataforma. É dividido em:
  * **Workspace clusters:** Onde os recursos computacionais da plataforma são hospedados no servidor da cloud usada por quem contrata o Databricks. 
  
  Eles se conectam com: **i. o storage da cloud; ii. o _databricks files system_ (_dbfs_); iii. e pode se comunicar com recursos externos.**

* ### **Control Plane:**
  É onde o Databricks opera através da cloud.<br/>
  Notebooks e outros objetos usados pelos usuários são armazenados aqui, sendo usados via site, API ou CLI.

  Seus serviços são:
  * **Web app:** Que fornece interface gráfica para o usuário operar os recursos da plataforma em alto nível. Com workspaces direcionados para as personas de **_analista_**, **_cientista_** e **_engenheiro de dados_**;
  * **Workflows Manager:** Que fornece jobs e Delta Live Tables (DLT) para orquestrar tarefas de várias maneiras;
  * **Cluster Manager:** Para ajudar na configuração e gerenciamento de clusters;
  * **Jobs:** Para agendar e monitorar tarefas;
  * **Unity Catalog:** Que permite a governança através do controle de acessos, gestão de metadados, linhagem e exploração de dados;
  * **Notebook, Repos e DBSQL:** Os recursos para desenvolvimento dentro do Databricks.<br/>
    São executados no **_data plane_**, esses por sua vez, gerenciados pelo **_control plane_**.

## **Compute resources:**

* ### **Cluster:**
  Coleção de instâncias de máquinas virtuais onde as cargas de trabalho dos _workspaces_ são distribuídas.<br/>
  Tipicamente, um _driver node_ está ligado a um ou mais _work nodes_. O driver que é responsável por distribuir a carga de trabalho entre os _work nodes_.<br/>
  * **Tipos de cluster:**
    * **_All purpose_:** 
      * Para desenvolvimento interativo e colaborativo, focado em notebooks.
      * É possível gerência-lo pela GUI do Databricks e API Rest.
      * Pode ser encerrado/iniciado de forma manual.
    * **_Job cluster_:**
      * Para ser usado em jobs e pipelines automatizadas de forma mais econômic e robusta.
      * Não é possível reiniciá-lo.

    A principal diferença entre os tipos de clusters é o tempo de retenção das suas configurações.<br/>
    O `all purpose` retém 30 dias de registros para 70 clusters. Os `job clusters` das 30 últimas execuções.

* ### **Configuração dos clusters:**

1. **Operacional:**
  * **Padrão (multinode):**
    * Requer no mínimo duas instâncias de máquinas virtuais (**_virtual machines - vm_**), para o driver e worker.
  * **Single Node:** 
    * Uma única vm hospedada no driver;
    * Não há instâncias de worker;
    * Tem custo menor do que a configuração padrão;
    * Ideal para:
      * Processos que fazem somente a carga e persistência dos dados
      * Processo de machine learning que usam apenas um nó
      * Análises exploratórias de poucos dados

2. **Databricks Runtime (DBR):**
  Uma coleção de componentes rodando em um cluster, divididos em:
  * **Padrão:** 
    `Apache Spark` e outros componentes e atualizações para otimização de análise em big data;
  * **`Photon`:**
    Um add-on para otimizar desempenho e custo de cargas SQL.
  * **Machine Learning:**
    Runtime aumentado específico para machine learning, com bibliotecas populares como TensorFlow, Keras, PyTorch e XGBoost.<br/>
  O `Unity Catalog` é disponível à partir da DBR 10.1, com algumas features lançadas posteriormente.

3. **Modo de seguração (security mode):**
  Antigo `Access Mode` (legado em 02/2023), especifica a segurança geral do cluster. Para clusters padrão há 4 opções de acesso:
  * **Single user:**, suportando todas as linguagens. Pode ser usado por um único usuário, designado a criar/editar o cluster;
  * **Shared:**, suportando `python`(à partir da DBR 11.1) e SQL, é recurso `premium` que permite o uso do cluster por vários usuários<br/>Recursos avançados como instalações de bibliotecas, InitScripts e criação de mounts são proibidos para garantir isolamento de segurança;
  * **No isolation shared:**, permitindo todas as linguagens, pode ser ocultado para adicionar o isolamento de usuário, ou definido da conta da pessoa<br/>Não tem suporte ao `Unity Catalog`;
  * **Custom:**, suporta todas as linguagens, é uma opção para clusters existentes sem algum dos modos de acesso permitidos atualmente. Não pode ser utilizado na criação de um novo cluster<br/>Não tem suporte ao `Unity Catalog`.

4. **Políticas de cluster:**
  Configurações no uso do Databricks definidas pelos usuários administradores.<br/>
  Controla as pessoas que podem se conectar, reiniciar, gerenciar o cluster.
