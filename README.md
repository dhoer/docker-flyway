# Docker Flyway

[![Docker Auto Build](https://img.shields.io/docker/automated/dhoer/flyway.svg?style=flat-square)][docker]

[docker]: https://hub.docker.com/r/dhoer/flyway/

Dockerized [flyway command-line tool](https://flywaydb.org/getstarted/firststeps/commandline) with options for updated mariadb driver or added mysql driver.

## Supported tags and respective `Dockerfile` links

-	[`4.0.3`, `4.0`, `4`, `latest` (*4.0/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/4/Dockerfile)
-	[`4.0.3-alpine`, `4.0-alpine`, `4-alpine`, `alpine` (*4.0/alpine/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/4/alpine/Dockerfile)
-	[`4.0.3-mariadb-1.5.3`, `4.0-mariadb-1.5`, `4-mariadb-1`, `mariadb` (*4-mariadb/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/4-mariadb/Dockerfile)
-	[`4.0.3-mariadb-1.5.3-alpine`, `4.0-mariadb-1.5-alpine`, `4-mariadb-1-alpine`, `mariadb-alpine` (*4-mariadb/alpine/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/4-mariadb/alpine/Dockerfile)
-	[`4.0.3-mysql-5.1.40`, `4.0-mysql-5.1`, `4-mysql-5`, `mysql-5`, `mysql` (*4-mysql/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/4-mysql/Dockerfile)
-	[`4.0.3-mysql-5.1.40-alpine`, `4.0-mysql-5.1-alpine`, `4-mysql-5-alpine`, `mysql-5-alpine`, `mysql-alpine` (*4-mysql/alpine/Dockerfile*)](https://github.com/dhoer/docker-flyway/blob/master/4-mysql/alpine/Dockerfile)

## Getting started

`docker run dhoer/flyway [flyway cli arguments here]`

## Example

Add a file named `V1__Initial.sql` with following contents:

```
CREATE TABLE MyTable (
    MyColumn VARCHAR(100) NOT NULL
);

```

### Linux
`docker run --rm -v $(pwd):/flyway/sql dhoer/flyway -url=jdbc:mysql/mydb -schemas=myschema -user=root -password=P@ssw0rd migrate`

### Windows 10 (Docker for Windows)
CMD:

`docker run --rm -v %cd%:/flyway/sql dhoer/flyway -url=jdbc:mysql/mydb -schemas=myschema -user=root -password=P@ssw0rd migrate`

PowerShell:

`docker run --rm -v C:\FolderWhereYourScriptsReside:/flyway/sql dhoer/flyway -url=jdbc:mysql/mydb -schemas=myschema -user=root -password=P@ssw0rd migrate`

### Windows 7 (Docker Toolbox)

`docker run --rm -v /c/Users/FolderWhereYourScriptsReside:/flyway/sql dhoer/flyway -url=jdbc:mysql/mydb -schemas=myschema -user=root -password=P@ssw0rd migrate`

### Example docker-compose.yml

A compose file that will setup a database container and a container to perform migrations.

```
version: '2'
services:
  schema:
    image: dhoer/flyway
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

Run `docker-compose up -d db`, wait a minute for MySQL to be initialized (or tail logs with `docker-compose logs -f`) then run `docker-compose up schema`.
