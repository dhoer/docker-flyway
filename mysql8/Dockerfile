FROM boxfuse/flyway:5.2.4

ENV MYSQL_DRIVER_VERSION 8.0.15

RUN curl -o drivers/mysql-connector-java-${MYSQL_DRIVER_VERSION}.jar \
    https://repo1.maven.org/maven2/mysql/mysql-connector-java/${MYSQL_DRIVER_VERSION}/mysql-connector-java-${MYSQL_DRIVER_VERSION}.jar
