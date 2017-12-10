FROM openjdk:8-jre

WORKDIR /flyway

ENV FLYWAY_VERSION 4.2.0

RUN curl -o flyway-commandline-${FLYWAY_VERSION}.tar.gz \
    https://repo1.maven.org/maven2/org/flywaydb/flyway-commandline/${FLYWAY_VERSION}/flyway-commandline-${FLYWAY_VERSION}.tar.gz \
  && tar -xzf flyway-commandline-${FLYWAY_VERSION}.tar.gz --strip-components=1 \
  && rm flyway-commandline-${FLYWAY_VERSION}.tar.gz \
  && rm drivers/mariadb-java-client-*.jar \
  && ln -s /flyway/flyway /usr/local/bin/flyway

ENV MARIADB_DRIVER_VERSION 1.7.0

RUN curl -o drivers/mariadb-java-client-${MARIADB_DRIVER_VERSION}.jar \
    https://repo1.maven.org/maven2/org/mariadb/jdbc/mariadb-java-client/${MARIADB_DRIVER_VERSION}/mariadb-java-client-${MARIADB_DRIVER_VERSION}.jar

ENTRYPOINT ["flyway"]
CMD ["--help"]
