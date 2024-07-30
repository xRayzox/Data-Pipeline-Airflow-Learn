### Learn-Airflow(Data-Pipeline-with-Airflow)
Data Pipeline with Airflow project using MinIO and Postgresql.

this project is refrence by github https://github.com/akarce/Udacity-Data-Pipeline-with-Airflow.git
## Project Structure:

+ dags: Directory containing Airflow DAG scripts.
    + create_tables_and_buckets.py: DAG for creating tables on PostgreSQL and buckets on MinIO.
    + create_tables.sql: SQL script containing CREATE TABLE queries.
    + pipeline_dag.py: Main DAG script for the ETL data pipeline.

+ data: Directory for storing project source data.
    + log_data: Subdirectory for log data.
    + song_data: Subdirectory for song data.

+ plugins: Directory for custom Airflow plugins.
    + operators: Subdirectory for operator scripts.
        + init.py: Initialization script for operators.
        + stage_postgresql_operator.py: Operator script for copying data from MinIO to PostgreSQL.
        + load_fact_operator.py: Operator script for executing INSERT queries into the fact table.
        + load_dimension_operator.py: Operator script for executing INSERT queries into dimension tables.
        + data_quality_operator.py: Operator script for performing data quality checks.

    + helpers: Subdirectory for helper scripts.
        + __init__.py: Initialization script for helpers.
        + sql_queries.py: Script containing SQL queries for building dimensional tables.

### Airflow Dag Overview

![alt text](images/dag_overview.png)

### Staging & Fact Table Schema

![alt text](images/erd1.png)

### Dimension Tables Schema

![alt text](images/erd2.png)

# How to Run

### Change directory

`$ cd Data-pipline-airflow`

## Build the docker images with docker compose:

`$ docker compose up -d`

### WebUI links: \
`Airflow webserver` http://localhost:8080/

`MinIO` http://localhost:9001/


### Sign in to Airflow webserver:

`Username:` airflow 
`Password:` airflow

![alt text](images/airflow_sign_in.png)


### Create Postgres connection:

#### Go to admin -> Connections -> Add a new record

Connection Id: postgres_conn \
Connection Type: Postgres \
Host: host.docker.internal \
Database: sparkifydb \
Login: postgres_user \
Password: postgres_password \
Port: 5432

![alt text](images/postgres_conn.png)


### Obtain Access Key from MinIO:

Go to MinIO WebUI using http://localhost:9001/ \
From Access Keys section -> create access key -> Create \
Store you Access Key and Secret Key using Download for Import

![alt text](images/create_access_key.png)


### Create MinIo connection:

#### Go to admin -> Connections -> Add a new record

Connection Id: minio_conn \
Connection Type: Amazon Web Services \
AWS Access Key ID: <your_key_id> \
AWS Secret Access Key: <your_secret_key> \

![alt text](images/minio_conn.png)



### Trigger the create_tables_and_buckets_dag using Airflow WebUI

![alt text](images/create_table_and_buckets.png)

This will create two buckets named \
    udacity-dend, processed \
and 7 postgres tables named \
    artists, songplays, songs, staging_events, staging_songs, times, users

![alt text](images/created_buckets.png)

![alt text](images/created_tables.png)

### To see the schemas of tables use \
`$ \d <table_name>`


![schemas](images/schemas.png)

### Upload the log_data and song_data folders from data folder into udacity-dend bucket from MinIO WebUI

`Username:` minioadmin \
`Password:` minioadmin

![Uploaded log_data and song_data from UI](images/uploaded_data.png)

### Finally unpause the dag named pipeline:

![alt text](images/unpause.png)

#### After finished successfully you can check the logs for data quality messages, and the all objects will be moved to the processed bucket.

![alt text](images/processed_bucket.png)


#### You can see the Airflow logs for data quality results:

![alt text](images/data_quality_result.png)

=======

![alt text](images/dag_overview.png)

### Staging & Fact Table Schema

![alt text](images/erd1.png)

### Dimension Tables Schema

![alt text](images/erd2.png)

# How to Run

## Clone the project repository

`$ git clone https://github.com/xRayzox/Data-Pipeline-Airflow-Learn.git`