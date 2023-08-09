# kafka-playground

Dette prosjektet viser bruk av Avro serialiseringsformat, og Confluent Schema Registry.

For lokal Kafka-broker, kjør `docker-compose up`.

Man kan starte en konsument i `kafka-connect`-containeren:

```sh
# På Docker host
docker exec -it kafka-connect bash

# Inne i kafka-connect container:
kafka-avro-console-consumer --bootstrap-server broker:29092 \
  --property schema.registry.url="http://schema-registry:8081" \
  --property print.key=true \
  --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
  --skip-message-on-error \
  --topic <topic>

# Hvis man ønsker å produsere meldinger for test
kafka-avro-console-producer --bootstrap-server broker:29092 \
  --property schema.registry.url="http://schema-registry:8081" \
  --property value.schema.id=1 \
  --topic <topic>
```

For å produsere meldinger (se kommando over), kan man angi meldinger i JSON-format, og avslutte med linjeskift.

Schema ID kan variere.
Du kan liste skjema i ditt schema registry med `curl http://localhost:9081/schemas`,
eventuelt fra _inne i containeren_ `curl http://schema-registry:8081/schemas`.
Finn `id` på skjema-innslaget du ønsker å bruke.
Dette må gjøres etter at applikasjonen har publisert en melding, og dermed registrert skjemaet.
