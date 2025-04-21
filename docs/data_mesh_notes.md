## Motivadores:
* Engenharia de dados como gargalo: Muita demanda para pouca gente e entregas com pouco valor (sem governança dos dados ou atuação apagando incêndio);
* Entrega de projetos, não produtos;
* Falta de ownership;
* Muito esforço para modelagem de dados. Dados formados à partir de tabelas criadas para outro propósito;
* Quebras constantes de schemas e pipelines. Modelagem acidental;
* Snapshots (fotos) em vez de fatos (filme). Sem histórico acurado dos dados;
* Silos por especialidades;
* Falta de contexto: Eventos encadeados para resultado visto em tela;
* Processo moroso e cheio de ruídos.

## Princípios Data Mesh:
* Dados orientados a domínios:
  Criar contexto dos dados, isolar eles para não virar uma "colcha de retalhos" (aplicação do DDD), ownership dos dados;
* Dados como produto:
  Monitorar, ser responsável, identificar usos não contemplados, entender o cliente e o consumidor
* Self-serve Data Platform:
  Abstrair complexidade na criação de dados analíticos. Proporcionar foco dos times no dado fornecido não no processo de fluxo desses dados. Escalas de atuação: infraestrutura, camada de alto nível, data products, etc..
* Governança Federada:
  Interoperabilidade e coesão dos processos de forma descentralizada (federada). Time de governança passa a ter um papel de orquestrar fóruns de governança federada ao invés de concentrar ações propriamente de governança

> PS: Data Mesh é gênero e arquitetura espécie

## Arquétipos de produtos:
* Source-alligned: Alinhados com o criador do produto;
* Aggregate: Produtos de diferentes domínios agregados (playlist sugerida pelo Spotify)
* Consumer-alligned: produtos destinados a um consumo específico (disparo de email, dashboard, ...)

## Principais desafios:
* Mudança cultural (Autonomia, maestria, propósito e metas);
* Dados já existentes (com diversas entregas de valor);
* Priorização (produtos do domínio X produto de dados);
* Plataformização.

## Domain Driven Design (DDD):
* Domínios: núcleos de conhecimento;
* Bounded context: agrupamentos dos domínios por contextos que façam sentido (contexto de vendas, de suportes, ...)
* Linguagem ubíqua: Linguagem definida dos produtos e sistemas (conceitos, aplicações, etc...)
