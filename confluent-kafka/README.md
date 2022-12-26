## Create Connector

### Permission Data

```
chown -R 1000:1000 zookeeper-data
chown -R 1000:1000 zookeeper-txn-logs
chown -R 1000:1000 kafka1-data
```

### Connector Source Postgres

```
{
  "name": "example-source-connector",
  "config": {
    "connector.class": "io.debezium.connector.postgresql.PostgresConnector",
    "tasks.max": "1",
    "database.hostname": "host.docker.internal",
    "database.port": "5433",
    "database.user": "administrator",
    "database.password": "k9gf5sfh18",
    "database.dbname" : "example",
    "database.server.name": "example",
    "topic.prefix": "example",
    "table.include.list": "public.person,public.customer",
    "transforms": "route",
    "transforms.route.type": "org.apache.kafka.connect.transforms.RegexRouter",
    "transforms.route.regex": "([^.]+)\\.([^.]+)\\.([^.]+)",
    "transforms.route.replacement": "$3",
    "time.precision.mode": "connect"
  }
}
```

### Connector Sink JDBC Postgres

```
{
  "name": "example-sink-connector",
  "config": {
    "connector.class": "io.confluent.connect.jdbc.JdbcSinkConnector",
    "tasks.max": "1",
    "connection.url": "jdbc:postgresql://host.docker.internal:5433/target",
    "connection.user": "administrator",
    "connection.password": "k9gf5sfh18",
    "input.data.format": "AVRO",
    "topics": "person,customer",
    "insert.mode": "upsert",
    "db.timezone": "UTC",
    "auto.create": "true",
    "auto.evolve": "true",
    "pk.mode": "record_value",
    "pk.fields": "id",
    "transforms": "unwrap,convert_created_at,convert_modified_at",
    "transforms.unwrap.type": "io.debezium.transforms.ExtractNewRecordState",
    "transforms.convert_created_at.type": "org.apache.kafka.connect.transforms.TimestampConverter$Value",
    "transforms.convert_created_at.target.type": "Timestamp",
    "transforms.convert_created_at.field": "created_at",
    "transforms.convert_created_at.format": "yyyy-MM-dd HH:mm:ss.SSSSSS",
    "transforms.convert_modified_at.type": "org.apache.kafka.connect.transforms.TimestampConverter$Value",
    "transforms.convert_modified_at.target.type": "Timestamp",
    "transforms.convert_modified_at.field": "modified_at",
    "transforms.convert_modified_at.format": "yyyy-MM-dd HH:mm:ss.SSSSSS"
  }
}
```
