# docker-flyway

[![Docker Auto Build](https://img.shields.io/docker/automated/dhoer/flyway.svg?style=flat-square)][docker]

[docker]: https://hub.docker.com/r/dhoer/flyway/


## Supported tags and respective `Dockerfile` links

-	[`4.0.3`, `4.0.3-mysql-5.1.39`, `latest` (*Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/Dockerfile)
-	[`4.0.3-alpine`, `4.0.3-mysql-5.1.39-alpine`, `alpine` (*alpine/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/alpine/Dockerfile)

## Getting started

`docker run aidevops/flyway [flyway cli arguments here]`

## Example

Add a file named `V1__Initial.sql` with following contents:
```
CREATE TABLE MyTable (
    MyColumn VARCHAR(100) NOT NULL
);

```

### Linux

`docker run --rm -v $(pwd):/flyway/sql aidevops/flyway -url=jdbc:mysql/mydb -schemas=myschema -user=root -password=P@ssw0rd migrate`


### Example docker-compose.yml

A compose file that will setup a database container and a container to perform migrations.

```
version: '2'
services:
  schema:
    image: aidevops/flyway
    command: -url=jdbc:mysql://db -schemas=myschema -user=root -password=P@ssw0rd migrate
    volumes:
      - .:/flyway/sql
    depends_on:
      - db
  db:
    image: mysql
    environment:
      - MYSQL_ROOT_PASSWORD=P@ssw0rd
    command: --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
    ports:
      - 3306:3306
```

Run `docker-compose up -d db`, wait a minute for MySQL to be initialized then run `docker-compose up schema`.
