# Postgres Change Data Capture
Postgres CDC using Debezium Kafka Connect as events

## 1. Local
- cd docker
- set the correct env var using .env.example
- docker compose up -d

## 2. Deployment
## 2.1 Dev
- Kafka Cluster
- Postgres Database

### 2.1.1 Kafka Topics

| Topic Name       | Topic Partition  | Topic CleanUp Policy | Topic Retention Period |
|------------------|------------------|----------------------|------------------------|
| `dz.configs`     | compact          | delete               | 7 days                 |
| `dz.offsets`     | compact          | delete               | 7 days                 |
| `dz.status`      | compact          | delete               | 7 days                 |

### 2.1.2 Table Connector Configs
- Kafka Topics
- Database Replication Slots

| Table Name                       | EH Name                               | EH Partition | EH CleanUp Policy | EH Retention Period | DB Slot Name                        | DB Publication Name                      |
|----------------------------------|---------------------------------------|--------------|-------------------|---------------------|-------------------------------------|------------------------------------------|
| <schema>.<table_name>            | `dz.<schema>.<table_name>`            | 5            | delete            | 7 days              | `<table_name>_dbz`                  | `<table_name>_connection_dbz`            |


### 2.1.3 Kafka Connect
- kafka connect config in [`docker/.env.example`](./docker/.env.example)
- sample table connector config templates in [`connector-config-template/table-connectors`](./connector-config-template/table-connectors/base_table_connector.json)

## 3. Debezium
### 3.1 Custom Debezium Image
- comment out lines in [`java.security`](./docker/java.security) and [`java.config`](./docker/java.config) due to issues with off-the-shelf `debezium/connect:2.3.0.Final` image connecting with Azure Postgres Flexible Server
  - Implemented Solution: https://stackoverflow.com/a/75827811 
