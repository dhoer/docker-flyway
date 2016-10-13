FROM openjdk:8-jre-alpine

# RUN addgroup flyway && adduser -D -G flyway flyway

RUN apk --no-cache add openssl

WORKDIR /flyway

ENV FLYWAY_VERSION 4.0.3

RUN wget https://repo1.maven.org/maven2/org/flywaydb/flyway-commandline/${FLYWAY_VERSION}/flyway-commandline-${FLYWAY_VERSION}.tar.gz \
  && tar -xzf flyway-commandline-${FLYWAY_VERSION}.tar.gz \
  && mv flyway-${FLYWAY_VERSION}/* . \
  && rm flyway-commandline-${FLYWAY_VERSION}.tar.gz \
  && sed -i 's/bash/sh/' /flyway/flyway \
#  && chown -R flyway:flyway /flyway \
  && ln -s /flyway/flyway /usr/local/bin/flyway

#USER flyway

ENTRYPOINT ["flyway"]
CMD ["--help"]
