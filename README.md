# Docker Flyway

[![Docker Auto Build](https://img.shields.io/docker/automated/dhoer/flyway.svg?style=flat-square)]( https://hub.docker.com/r/dhoer/flyway/) [![travis](http://img.shields.io/travis/dhoer/docker-flyway/master.svg?style=flat-square)](https://travis-ci.org/dhoer/docker-flyway)

Extends [Official Flyway Command-line Docker images](https://github.com/flyway/flyway-docker) to use MySQL Connector/J driver instead of MariaDB Connector/J driver for `jdbc:mysql:` connections.

## Supported tags and respective `Dockerfile` links

- [`5.2.4-mysql-8.0.15`, `5.2-mysql-8.0`, `5-mysql-8`, `mysql8`, `mysql`, `latest` (*mysql8/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/mysql8/Dockerfile)
- [`5.2.4-mysql-8.0.15-alpine`, `5.1-mysql-8.0-alpine`, `5-mysql-8-alpine`, `mysql8-alpine`, `mysql-alpine`, `alpine` (*mysql8/alpine/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/mysql8/alpine/Dockerfile)
- [`5.2.4-mysql-5.1.47`, `5.2-mysql-5.1`, `5-mysql-5`, `mysql5` (*mysql5/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/mysql5/Dockerfile)
- [`5.2.4-mysql-5.1.47-alpine`, `5.2-mysql-5.1-alpine`, `5-mysql-5-alpine`, `mysql5-alpine` (*mysql5/alpine/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/mysql5/alpine/Dockerfile)

## Getting started

`docker run dhoer/flyway:alpine [flyway cli arguments here]`

## Example

Add a file named `V1__Initial.sql` with following contents:

    CREATE TABLE MyTable (
        MyColumn VARCHAR(100) NOT NULL
    );

### Linux

    docker run --rm -v $(pwd):/flyway/sql dhoer/flyway:alpine -url=jdbc:mysql://mydb -schemas=myschema -user=root -password=P@ssw0rd migrate

### Windows 10 (Docker for Windows)

CMD:

    docker run --rm -v %cd%:/flyway/sql dhoer/flyway:alpine -url=jdbc:mysql://mydb -schemas=myschema -user=root -password=P@ssw0rd migrate

PowerShell:

    docker run --rm -v C:\FolderWhereYourScriptsReside:/flyway/sql dhoer/flyway:alpine -url=jdbc:mysql://mydb -schemas=myschema -user=root -password=P@ssw0rd migrate

### Windows 7 (Docker Toolbox)

    docker run --rm -v /c/Users/FolderWhereYourScriptsReside:/flyway/sql dhoer/flyway:alpine -url=jdbc:mysql://mydb -schemas=myschema -user=root -password=P@ssw0rd migrate

### Example docker-compose.yml

To run both Flyway and the database that will be migrated in containers, you can use a `docker-compose.yml` file that starts and links both containers.

    version: '3'
    services:
      flyway:
        image: dhoer/flyway:alpine
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

Run `docker-compose up -d db`, wait a minute for MySQL to be initialized (or tail logs with `docker-compose logs -f`) then run `docker-compose up flyway`.
