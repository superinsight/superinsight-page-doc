
## What is ML Database
ML database works like a traditonal relational database and on top of that, it also allow you to make ML predictions using SQL query.

## Creating a ML Database
Click on the `[Create Database]` button to create a new ML database.

## Attaching Other Databases
Click on the `[Attach Database]` button and connect to one of our supported databases using their connection string

| Databases             | Connection String                    |
| -----------           | ------------------------------------ |
| AWS Redshift          | `redshift+psycopg2://<userName>:<DBPassword>@<AWS End Point>:5439/<Database Name>`  |
| ClickHouse            | `clickhouse+native://{username}:{password}@{hostname}:{port}/{database}` |
| ElasticSearch         | `elasticsearch+http://{user}:{password}@{host}:9200` |
| BigQuery              | `bigquery://{project_id}` |
| MySQL                 | `mysql://<UserName>:<DBPassword>@<Database Host>/<Database Name>` |
| PostgreSQL            | `postgresql://<UserName>:<DBPassword>@<Database Host>/<Database Name>` |
| Snowflake             | `elasticsearch+http://{user}:{password}@{host}:9200` |