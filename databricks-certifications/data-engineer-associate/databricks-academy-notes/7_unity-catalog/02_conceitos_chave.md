## 1. **Key concepts:**

### 1.1. **Componentes:**
* **Metastore**
    * **Catalog**
        * **Schema**
            * **Table**
            * **View**
            * **Function**

### 1.2. **External Storage**
Composta de Storage credentials e external locations, são elementos da cloud que podem ser usados pelo Databricks

### 1.3. **Delta Sharing**
Composta de Shares e recipients, são espaços do Databricks que podem ser disponibilizados para usuários de fora da empresa.

## 2. **Arquitetura:**

É baseada em separar os recursos computacionais nos workspaces, e os recursos do Databricks (gestão de usuários e grupos, Metastore, Controle de Acessos) no Unity Catalog.
Permite que múltiplos workspaces compartilhem o mesmo controle de acessos.

## 3. **Recursos:**

### 3.1. **Cluster access mode:**
Suporta o single user e shared, não suporta o no isolation shared

### 3.2. **Papéis:**
1. **Cloud Admin:** Gerencia os recursos da cloud
2. **Identity admin:** Gerencia usuários e grupos no provedor de identidade
3. **Account admin:** Gerencia os grupos criados anteriormente, Tem acesso total aos objetos de dados, CRUD dos metastores

## 4. **Metastore admin:**
CRUD e gestão de privilégios nos catálogos que o compõem

## 5. **Data owner:**    
Dono dos dados que criou. Pode criar objetos aninhados a ele e gere as permissões dos seus objetos

## 6. **Workspace admin:** 
Gerencia acessos, jobs, clusters usuários e privilégios do seu workspace
