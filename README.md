# Google Cloud JDS Research

## Config

### Nginx

`nginx/conf.d/example.com`

```
upstream <name> {
  	server <ip>:<port>;
}

server {
	if ($host = <domain>) {
		return 301 https://$host$request_uri;
	}

	listen 80;

	server_name <domain>;

	location /.well-known/acme-challenge/ {
		root /var/www/certbot;
	}
}

server {
	listen 443 ssl;

	server_name <domain>;

	ssl_certificate /etc/nginx/ssl/live/<domain>/fullchain.pem;
	ssl_certificate_key /etc/nginx/ssl/live/<domain>/privkey.pem;

	location / {
		proxy_pass http://<name>/;
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
docker exec -ti nginx bash
nginx -t
```

### Certbot

```
docker-compose -f nginx/certbot.yml run --rm certbot certonly --webroot --webroot-path /var/www/certbot/ -d example.com
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
