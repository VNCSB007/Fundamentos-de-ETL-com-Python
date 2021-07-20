# Introdução ao ETL 

Uma forma simples de se observar o ETL é na extração da fonte dos dados, transformação para que estes sejam legíveis e carregamento desses dados legíveis para Data Warehouse

## Extract, Transform and Load

**Extração:** os dados são extraídos das mais diversas fontes de dados (banco de dados, arquivos, API; processadas em lote ou em tempo real)

Mas eles podem vir de forma inconsistente, e como Engenheiro de dados é preciso garantir a sua integridade

**Transformação:** vamos valida-los, limpa-los, para que assim se padronize a forma de analisa-los e estuda-los

**Load:** carregados ao Data Warehouse

## Qual a necessidade?

**Data Base, Excel, text, PDF, API (como Facebook, Twitter, Tumblr, Google, Sites em geral [afinal, é Application Programming Interface]), sensores como câmeras ou sensores termais, conexões via Bluetooth ou Wi-Fi**; a questão aqui é: como conciliar tudo isso para deixar fácil a análise?

Todos esses dados precisam ser extraídos e organizados para serem trabalhados de forma organizada. Podem ser organizados em lote (_Batch_) ou em tempo real (_Real Time_) 

A transformação é necessária para todos os dados serem normalizados, validados e que possam, de forma íntegra, serem carregados para um armazém de dados (_Data Warehouse_) para ser gerado uma decisão precisa em relação aos dados reais

O carregamento dentro das nossas bases de dados para que possam ser relacionados as informações e, com programas de base de dados relacionais ou não, permitiram que a equipe de ciência de dados possa decidir 

## Visão Técnica Geral da ETL

Todo o ETL, desde a extração até o seu armazenamento para a participação em análise de dados, participa de um pedaço do que é considerado como _Pipeline_ ou _Data Flow_ (o caminho dos dados desde a sua obtenção até os resultados em visualizadores de dados como o Power BI ou Tableau).

É importante ressaltar que caso uma validação de dados chegue em um dado estranho (30 de fevereiro; pessoa com 200 anos; devedor com bens novos sem novos empréstimos no banco) cabe à instituição ou à empresa decidir se o dado deve ser deletado, alterado para a possibilidade mais próxima, extraído novamente, questionado em sua fonte, verificado como novo precedente, entre outras decisões.

Importante ressaltar que o Pipeline em si começa com a junção do Data source com o Data Validation, no mínimo. C.c., é apenas obtenção de dados; tal qual, computar os dados para visualização não faz aprte necessariamente do pipeline, mas auxiliar no Data Loading e ai sim se aproveitar das ferramentas do BI results seria considerado parte do Data Flow.
### PIPELINE

_Data Source_: diferentes fontes de dados, como OS (Operational System), ERP (Enterprise Resource Planning), CRM (Customer Relationship Management), SQL (DB baseado em Structured Query Language), _flat files_ ou _spredsheets_, [...]

| ***ETL:*** Data Validation > Data Cleaning > Data Transforming > Data Aggregation > Data Loading

##### DATA WAREHOUSE | 

E então os dados estão prontos para análise através de ferramentas _BI results:_ Análises OLAP (Online Analytical Processing), Mineração de Dados (_Data Mining_), Visualização de Dados (_Data Visualization_), Relatórios (_Reports_), Painéis de análise (_Dashboards_), Alertas (_Alerts_)

## Ferramentas e Pacotes

ferramentas / pacotes para Python :

- Airflow Apache (gerenciamento de _WorkFlows_ para intervir com programação)
- Luigi (facilita construção de ferramentas de visualização, reparação de falhas; Bom para otimizar processos de ETL)
- Bonobo (executar processos em paralelo; extrai de CSV, JSON, XML, entre outros)
- Bubbles (compreensão e transparência do ETL)
- Petl (uso geral, facilita o uso na linguagem Python e construção do processo todo; fácil de trabalhar, mas não foi projetado para pipelines de grande memória requerida)
- Pandas (parecido com o Petl, mas os quadros de dados pode ser utilizado para uma maior Batch ou Streaming de dados)
- [...]

Links importantes!

http://airflow.apache.org/

http://bubbles.databrewery.org/

https://luigi.readthedocs.io/en/stable/

https://www.bonobo-project.org/

https://petl.readthedocs.io/en/stable/

Aquele que iremos utilizar nesse curso:

https://pandas.pydata.org/



Referências:

- A proposed model for data warehouse ETL processes. Journal of King Saud University - Computer and information Sciences, 2011
- Why You Need ETL Testing and What You Need To Know. Link: https://www.cigniti.com/blog/why-need-etl-testing-what-need-now/



