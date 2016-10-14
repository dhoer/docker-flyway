FROM openjdk:8-jre-alpine

RUN apk --no-cache add openssl

WORKDIR /flyway

ENV FLYWAY_VERSION 4.0.3

RUN wget https://repo1.maven.org/maven2/org/flywaydb/flyway-commandline/${FLYWAY_VERSION}/flyway-commandline-${FLYWAY_VERSION}.tar.gz \
  && tar -xzf flyway-commandline-${FLYWAY_VERSION}.tar.gz \
  && mv flyway-${FLYWAY_VERSION}/* . \
  && rm flyway-commandline-${FLYWAY_VERSION}.tar.gz \
  && rm drivers/mariadb-java-client-*.jar \
  && sed -i 's/bash/sh/' /flyway/flyway \
  && ln -s /flyway/flyway /usr/local/bin/flyway

ENV MARIADB_DRIVER_VERSION 1.5.3

RUN wget https://repo1.maven.org/maven2/org/mariadb/jdbc/mariadb-java-client/${MARIADB_DRIVER_VERSION}/mariadb-java-client-${MARIADB_DRIVER_VERSION}.jar \
  && mv mariadb-java-client-${MARIADB_DRIVER_VERSION}.jar drivers

ENTRYPOINT ["flyway"]
CMD ["--help"]
