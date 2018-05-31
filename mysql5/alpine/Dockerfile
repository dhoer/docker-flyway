FROM boxfuse/flyway:5.1.1-alpine

ENV MYSQL_DRIVER_VERSION 5.1.46

RUN wget -O drivers/mysql-connector-java-${MYSQL_DRIVER_VERSION}.jar \
    https://repo1.maven.org/maven2/mysql/mysql-connector-java/${MYSQL_DRIVER_VERSION}/mysql-connector-java-${MYSQL_DRIVER_VERSION}.jar
