# Docker Flyway

[![Docker Auto Build](https://img.shields.io/docker/automated/dhoer/flyway.svg?style=flat-square)][docker]

[docker]: https://hub.docker.com/r/dhoer/flyway/

Official [Flyway Command-line Docker images](https://github.com/flyway/flyway-docker) are now available!

Extends Flyway Command-line Docker images with options to use:
- MySQL Connector/J driver instead of MariaDB Connector/J driver for jdbc:mysql: connections
- MariaDB Connector/J driver version newer than the one that ships with flyway (v4.2.0 only since v5.0.2 has latest driver)

## Supported tags and respective `Dockerfile` links

- [`5.0.2-mysql-5.1.45`, `5.0-mysql-5.1`, `5-mysql-5`, `mysql-5`, `mysql` (*5-mysql/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/5-mysql/Dockerfile)
- [`5.0.2-mysql-5.1.45-alpine`, `5.0-mysql-5.1-alpine`, `5-mysql-5-alpine`, `mysql-5-alpine`, `mysql-alpine` (*5-mysql/alpine/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/5-mysql/alpine/Dockerfile)

- [`4.2.0-mysql-5.1.45`, `4.2-mysql-5.1`, `4-mysql-5` (*4-mysql/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/4-mysql/Dockerfile)
- [`4.2.0-mysql-5.1.45-alpine`, `4.2-mysql-5.1-alpine`, `4-mysql-5-alpine` (*4-mysql/alpine/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/4-mysql/alpine/Dockerfile)
- [`4.2.0-mariadb-1.7.0`, `4.2-mariadb-1.7`, `4-mariadb-1`, `mariadb` (*4-mariadb/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/4-mariadb/Dockerfile)
- [`4.2.0-mariadb-1.7.0-alpine`, `4.2-mariadb-1.7-alpine`, `4-mariadb-1-alpine`, `mariadb-alpine` (*4-mariadb/alpine/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/4-mariadb/alpine/Dockerfile)

## Getting started

`docker run dhoer/flyway:mysql [flyway cli arguments here]`

## Example

Add a file named `V1__Initial.sql` with following contents:

```
CREATE TABLE MyTable (
    MyColumn VARCHAR(100) NOT NULL
);

```
                                                             
### Linux
`docker run --rm -v $(pwd):/flyway/sql dhoer/flyway:mysql -url=jdbc:mysql://mydb -schemas=myschema -user=root -password=P@ssw0rd migrate`

### Windows 10 (Docker for Windows)
CMD:

`docker run --rm -v %cd%:/flyway/sql dhoer/flyway:mysql -url=jdbc:mysql://mydb -schemas=myschema -user=root -password=P@ssw0rd migrate`

PowerShell:

`docker run --rm -v C:\FolderWhereYourScriptsReside:/flyway/sql dhoer/flyway:mysql -url=jdbc:mysql://mydb -schemas=myschema -user=root -password=P@ssw0rd migrate`

### Windows 7 (Docker Toolbox)

`docker run --rm -v /c/Users/FolderWhereYourScriptsReside:/flyway/sql dhoer/flyway:mysql -url=jdbc:mysql://mydb -schemas=myschema -user=root -password=P@ssw0rd migrate`

### Example docker-compose.yml

To run both Flyway and the database that will be migrated in containers, you can use a `docker-compose.yml` file that starts and links both containers.

```
version: '3'
services:
  flyway:
    image: dhoer/flyway:mysql
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

Run `docker-compose up -d db`, wait a minute for MySQL to be initialized (or tail logs with `docker-compose logs -f`) then run `docker-compose up flyway`.
