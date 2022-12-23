## Create Connector

### Connector Postgres

```
{
  "name": "example-connector",
  "config": {
    "connector.class": "io.debezium.connector.postgresql.PostgresConnector",
    "database.hostname": "host.docker.internal",
    "database.port": "5433",
    "database.user": "administrator",
    "database.password": "password",
    "database.dbname" : "example",
    "topic.prefix": "example",
    "table.include.list": "public.person,public.customer"
  }
}
```
