# BD ETL Automation with Airflow

Este repositório contém o trabalho final da disciplina de Big Data, que consiste na automação de processos ETL (Extract, Transform, Load) utilizando Apache Airflow.

## Propósito da Solução

A solução visa automatizar o processo de coleta e processamento de dados de preços de ações. Os dados são extraídos de uma API, armazenados no Minio, formatados utilizando Spark e, finalmente, carregados em um data warehouse para análise posterior.

## Descrição da Arquitetura

A arquitetura do projeto é composta pelos seguintes componentes:

1. **Airflow DAG**: Orquestra o fluxo de trabalho, definindo as tarefas a serem executadas e suas dependências.
2. **Minio**: Utilizado como armazenamento de objetos para guardar os dados de preços de ações.
3. **Spark**: Utilizado para formatar os dados de preços de ações.
4. **Postgres**: Utilizado como data warehouse para armazenar os dados formatados.

### Fluxo de Trabalho (DAG)

1. **is_api_available**: Verifica se a API de dados de ações está disponível.
2. **get_stock_prices**: Extrai os preços de ações da API.
3. **store_prices**: Armazena os dados de preços de ações no Minio.
4. **format_prices**: Formata os dados de preços de ações utilizando um contêiner Docker.
5. **get_formatted_csv**: Obtém o arquivo CSV formatado a partir do Minio.
6. **load_to_dw**: Carrega os dados formatados no data warehouse Postgres.

## Como Testar

### Passo a Passo

1. Clone o repositório e navegue até o diretório do projeto:
   ```bash
   git clone https://github.com/lfbitencourt/bd-etl-automation-airflow
   renomeie o nome da pasta para airflow
   cd airflow
   ´´´
2. Inicie os contêineres usando o Astro:
     ```bash
        astro dev start
    ´´´
3. Acesse a interface do Airflow em seu navegador e configure as conexões necessárias:

    minio: Conexão para o Minio.
    stock_api: Conexão para a API de dados de ações.
    postgres: Conexão para o data warehouse Postgres.
    Estrutura da Conexão no Airflow

    minio:

    Conn Id: minio
    Conn Type: Amazon Web Services
    Key_ID: minio
    secret_access Key ID: minio123
    Extra: {"endpoint_url": "http://host.docker.internal:9000"}
    
    stock_api:

    Conn Id: stock_api
    Conn Type: HTTP
    Host: https://query1.finance.yahoo.com/
    Extra: {"endpoint": "v8/finance/chart/",
            "headers": {
                "Content-Type": "application/json",
                "User-Agent": "Mozilla/5.0"
            }}
    
    postgres:

    Conn Id: postgres
    Conn Type: Postgres
    Host: postgres
    Login: postgres
    Password: postgres
    Port: 5432
    Extra: {}


4. Crie uma imagem airflow/stock-app
    ```bash
        cd spark/notebooks/stock_transform/
        docker build . -t airflow/stock-app
    ´´´

5. Inicie a DAG stock_market na interface do Airflow.

6. Monitore a execução das tarefas na interface do Airflow e verifique os resultados no Minio e no Postgres.
