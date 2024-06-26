### **1. O que é:**
	Um projeto open source que permite construir um data Lakehouse em um cloud storage existente

### **Delta é o default do Databricks**

### **1.1. Características:**
* Construído sobre formatos de dados padronizados (parquet e  json)
* Otimizado para armazenamento de objetos em cloud
* Construindo para lidar com metadados escaláveis

### **2. Warehouse like**
Delta Lake se preocupa em trazer os princípios ACID para o contexto do Data lake, assim garantindo:
* **Atomicidade:** Todas as transações devem ser bem sucedidas ou falharem completamente
* **Consistência:** O dado terá o mesmo estado se observado simultaneamente
* **Isolamento:** Como operações simultâneas confeitarão entre si
* **Durabilidade:** Mudanças registradas são permanentes

### **3. Problemas endereçados com ACID:**
* Append de data
* Modificação de data existente
* Job falhando durante sua execução
* Dificuldade de operações real-time
* Custo de manter histórico da versão dos dados
