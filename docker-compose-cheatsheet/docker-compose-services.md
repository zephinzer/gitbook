# Docker Compose Services

## Overview

This page provides YAML configurations for services I often use via a Docker Compose file usually as part of bringing up a development enviornment.

Some overarching rules for the following manifests are:

1. All usernames will be `user` unless setting it as such is impossible
2. All passwords will be `password` unless setting it as such is impossible
3. Persistence for data is enabled by default will be on local disk at a directory `./data` and will follow the image's recommended configuration path
4. Configuration for services is done via environment variables as far as possible
5. Configuraton for services which have to be via file will be on local disk at a directory `./config/%SERVICE_NAME%/*`
6. Ports will always be forwarded from container to host AS-IS, this means if you have another instance of the same service running locally, the port will fail to listen on the host network. Configure as needed

## Services

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
      - ./.data/var/lib/mysql/data:/var/lib/mysql
```

### PostgreSQL

```yaml
version: "3.7"
services:
  postgresql:
    # image reference: https://hub.docker.com/_/postgres
    image: postgres:13.2-alpine
    environment:
      POSTGRES_PASSWORD: password
      POSTGRES_USER: user
      POSTGRES_DB: database
    ports: ["5432:5432"]
    volumes: # [] # uncomment this and comment below to remove persistence
      - ./.data/var/lib/postgresql/data:/var/lib/postgresql/data
```

### Redis

```yaml
version: "3.7"
services:
  redis:
    # image reference: https://hub.docker.com/_/redis
    image: redis:6.2.2-alpine
    command:
      - redis-server
      - /usr/local/etc/redis/redis.conf
    ports: ["6379:6379"]
    volumes: # [] # uncomment and comment below to remove persistence
      - ./.config/redis/redis.conf:/usr/local/etc/redis/redis.conf
      - ./.data/redis/data:/data
```

#### Sample configuration file

```text
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

