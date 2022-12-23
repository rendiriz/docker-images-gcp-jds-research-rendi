# Google Cloud JDS Research

## Config

### Nginx

`nginx/conf.d/example.com.conf`

```
upstream <upstream_name> {
  server <ip>:<port>;
}

server {
  listen 80;
  listen [::]:80;

  server_name <domain> www.<domain>;
  server_tokens off;

  location /.well-known/acme-challenge/ {
    root /var/www/certbot;
  }

  location / {
    return 301 https://<domain>$request_uri;
  }
}

server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;

  server_name <domain>;

  ssl_certificate /etc/nginx/ssl/live/<domain>/fullchain.pem;
  ssl_certificate_key /etc/nginx/ssl/live/<domain>/privkey.pem;

  location / {
    proxy_pass http://<upstream_name>/;
    proxy_set_header Host $host;
  }
}
```

## Command

### Docker

```
docker images
docker ps -a
```

### Nginx

```
docker-compose -f nginx/docker-compose.yml up -d
docker-compose -f nginx/docker-compose.yml down
docker nginx restart
```

### Certbot

```
docker-compose -f nginx/certbot.yml run --rm certbot certonly --webroot --webroot-path /var/www/certbot/ -d example.com
docker-compose -f nginx/certbot.yml run --rm certbot renew
```

### Portainer

```
docker-compose -f portainer/docker-compose.yml up -d
docker-compose -f portainer/docker-compose.yml down
```

### Meilisearch

```
docker-compose -f meilisearch/docker-compose.yml --env-file meilisearch/.env up -d
docker-compose -f meilisearch/docker-compose.yml down
```

### PostgreSQL

```
docker-compose -f postgresql/docker-compose.yml --env-file postgresql/.env up -d
docker-compose -f postgresql/docker-compose.yml down
```

### Debezium PostgreSQL

```
docker-compose -f debezium/docker-compose.yml --env-file debezium/.env up -d
docker-compose -f debezium/docker-compose.yml down
```

### Debezium Kafka

```
docker-compose -f debezium-kafka/docker-compose.yml up -d
docker-compose -f debezium-kafka/docker-compose.yml down
```

## Docker Image Library

### Nginx

- nginx:1.23.1

### Certbot

- certbot/certbot:latest

### Portainer

- portainer/portainer-ce:2.15.1
- portainer/agent:2.15.1

### Meilisearch

- getmeili/meilisearch:v0.29

### PostgreSQL

- postgres:14.5

### Debezium PostgreSQL

- debezium/postgres:14

### Debezium Kafka

- debezium/zookeeper:2.1
- debezium/kafka:2.1
- debezium/connect:2.1
- obsidiandynamics/kafdrop:3.30.0
