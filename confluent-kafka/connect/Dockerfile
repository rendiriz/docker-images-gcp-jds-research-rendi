ARG image
FROM ${image}

## Install connectors
RUN echo "\nInstalling all required connectors...\n" && \
  confluent-hub install --no-prompt debezium/debezium-connector-postgresql:1.9.7 && \
  confluent-hub install --no-prompt confluentinc/kafka-connect-jdbc:10.6.0