---
description: >-
  This page provides YAML configurations for services I often use via a Docker
  Compose file usually as part of bringing up a development enviornment.
---

# 🍂 Docker Compose Services

## Overview

Some overarching rules for the following manifests are:

1. All usernames will be `user` unless setting it as such is impossible
2. All passwords will be `password` unless setting it as such is impossible
3. Persistence for data is enabled by default will be on local disk at a directory `./data` and will follow the image's recommended configuration path
4. Configuration for services is done via environment variables as far as possible
5. Configuraton for services which have to be via file will be on local disk at a directory `./config/%SERVICE_NAME%/*`
6. Ports will always be forwarded from container to host AS-IS, this means if you have another instance of the same service running locally, the port will fail to listen on the host network. Configure as needed

## Services

### Alpine

```yaml
version: "3.7"
services:
  alpine:
    # image reference: https://hub.docker.com/_/alpine
    image: library/alpine:3.13.5
    entrypoint: ["sleep", "1000000"]
```

### MongoDB

```yaml
version: "3.7"
services:
  mongodb:
    # image reference: https://hub.docker.com/_/mongo
    image: library/mongo:5.0.2
    environment:
      MONGO_INITDB_DATABASE: database
      MONGO_INITDB_ROOT_USERNAME: user
      MONGO_INITDB_ROOT_PASSWORD: password
    ports: ["27017:27017"]
    volumes: # [] # uncomment this and comment below to remove persistence
      - ./.data/mongodb/data/db:/data/db
```

### MySQL

```yaml
version: "3.7"
services:
  mysql:
    # image reference: https://hub.docker.com/_/mysql
    image: library/mysql:5.7.34
    environment:
      MYSQL_PASSWORD: password
      MYSQL_USER: user
      MYSQL_DATABASE: database
    ports: ["3306:3306"]
    volumes: # [] # uncomment this and comment below to remove persistence
      - ./.data/mysql/var/lib/mysql/data:/var/lib/mysql
```

### PostgreSQL

```yaml
version: "3.7"
services:
  postgresql:
    # image reference: https://hub.docker.com/_/postgres
    image: library/postgres:13.3-alpine
    environment:
      POSTGRES_PASSWORD: password
      POSTGRES_USER: user
      POSTGRES_DB: database
    ports: ["5432:5432"]
    volumes: # [] # uncomment this and comment below to remove persistence
      - ./.data/postgresql/var/lib/postgresql/data:/var/lib/postgresql/data
```

### Nginx

```yaml
version: "3.7"
services:
  nginx:
    # image reference: https://hub.docker.com/_/nginx
    image: library/nginx:1.21.0-alpine
    environment:
      NGINX_HOST: localhost
      NGINX_PORT: 3000
    ports: ["3000:80"]
    volumes:
      - ./.data/html:/usr/share/nginx/html
```

### Redis

```yaml
version: "3.7"
services:
  redis:
    # image reference: https://hub.docker.com/_/redis
    image: library/redis:6.2.2-alpine
    command:
      - redis-server
      - /usr/local/etc/redis/redis.conf
    ports: ["6379:6379"]
    volumes: # [] # uncomment and comment below to remove persistence
      - ./.config/redis/redis.conf:/usr/local/etc/redis/redis.conf
      - ./.data/redis/data:/data
```

#### Sample Redis 6 configuration file

```
# security configurations as documented at https://redis.io/topics/security
bind 0.0.0.0
rename-command CONFIG ""

# disable default user
requirepass password
user default off -@all

# setup appserver user
# to generate the password, run `printf -- 'password' | sha256sum | cut -f 1 -d ' '`
# the following password (after the '#' character) is the sha256 of "password" without the quotes
user user on ~* +ping +@read +@write +@set +@list #5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8
```

### Rundeck

```yaml
version: "3.7"
services:
  rundeck:
    # image reference: https://hub.docker.com/r/rundeck/rundeck
    image: rundeck/rundeck:3.3.11
    # environment reference: https://docs.rundeck.com/docs/administration/configuration/docker.html
    environment:
      RUNDECK_GRAILS_URL: http://localhost:4440
      RUNDECK_SECURITY_HTTPHEADERS_ENABLED: "false"
    ports: ["4440:4440"]
    volumes: # [] # uncomment this and comment below to remove persistence
      # you will have to run `sudo chmod 777 ./.data/rundeck/home/rundeck/server/data`
      - ./.data/rundeck/home/rundeck/server/data:/home/rundeck/server/data
```
