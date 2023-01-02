ARG image
FROM ${image}

RUN apt-get update -yqq && apt-get -yqq upgrade && apt-get install -yqq wget

WORKDIR /app

RUN wget https://repo1.maven.org/maven2/io/debezium/debezium-connector-cassandra/1.9.0.Beta1/debezium-connector-cassandra-1.9.0.Beta1.jar

COPY default_cassandra.yaml /opt/cassandra/cassandra.yaml

RUN mkdir /etc/debezium
RUN mkdir -p /tmp/debezium/offsets
RUN mkdir -p /tmp/debezium/relocation

ADD start-debezium.sh start-debezium.sh

RUN chmod 777 start-debezium.sh