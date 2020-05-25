# ElasticStack with SSL/TLS Enabled Docker Containers using Docker-Compose
Encrypting communications in an Elasticsearch Docker Container

### Steps to configure:

- Export below Environment variables
```bash
export COMPOSE_PROJECT_NAME="es"
export CERTS_DIR="/usr/share/elasticsearch/config/certificates"
export VERSION="7.7.0"
```

Make sure Docker Engine is allocated with least 4GiB of memory. In Docker Desktop, you configure resource usage on the Advanced tab 

- Command to generate certificate:
```bash
docker-compose -f create-certs.yml run --rm create_certs
```

- Bring up the entire stack:
```bash
docker-compose up -d
```

- To generate random passwords for all users:

```bash
docker exec elastic /bin/bash -c "bin/elasticsearch-setup-passwords \
auto --batch \
--url https://localhost:9200"
```


Copy all the passwords generated and keep it at some safe location.
Change the docker-compose.yml file with the password generated for kibana user:

docker-compose.yml
```
ELASTICSEARCH_PASSWORD: ****CHANGEME****
```
Similarly change the config/filebeat.yml file with the password generated for elastic user

config/filebeat.yml
```
password: ****CHANGEME****
```

Restart the containers after the password update with the following command

```bash
docker-compose stop && docker-compose up -d
```

- Elasticsearch can be accessed now from below link and you wil be prompted for username and password(use elastic user):
```
https://localhost:9200/_cat/indices?v
```
Kibana can be accessed now from below link and you wil be prompted for username and password(use elastic user):
```
https://localhost:5601/
```

- To remove all docker resources:
```bash
docker-compose down
```
