# elasticstack-tls
Encrypting communications in an Elasticsearch Docker Container

### Steps to configure:

- Export below Environment variables
```bash
COMPOSE_PROJECT_NAME=es
CERTS_DIR=/usr/share/elasticsearch/config/certificates
VERSION=7.6.2
```

Make sure Docker Engine is allotted at least 4GiB of memory. In Docker Desktop, you configure resource usage on the Advanced tab 

- Command to generate certificate:
```bash
docker-compose -f create-certs.yml run --rm create_certs
```

- Bring up the entire stack:
```bash
docker-compose -f elastic-docker-tls.yml up -d
```

- To generate random passwords for all users:

```bash
docker exec es01 /bin/bash -c "bin/elasticsearch-setup-passwords \
auto --batch \
--url https://localhost:9200"
```


Copy all the passwords generated and keep it at some safe location.
Change below config files with the password generated for elastic user:

elastic-docker-tls.yml
```
ELASTICSEARCH_PASSWORD: ****CHANGEME****
```
and default.conf

```
output {
    elasticsearch {
		hosts => "https://es01:9200"
        cacert => '/usr/share/elasticsearch/config/certificates/ca/ca.crt'
        user     => 'elastic'
        password => *****"CHANGEME"**** 
        ssl => true
        ssl_certificate_verification => false
	}
```

- Elasticsearch can be accessed now from below link and you wil be prompted for username and password:
```
https://localhost:9200/_cat/indices?v
```
Kibana can be accessed now from below link and you wil be prompted for username and password:
```
https://localhost:5601/
```

- To remove all docker resources:
```bash
docker-compose -f elastic-docker-tls.yml up
```
