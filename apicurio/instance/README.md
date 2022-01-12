# Apicur.io 2.x deployment

## Operator

```sh
oc apply -k apicurio/operator/overlays/stable
```

## Operand

For Event Streams as Kafka backend

```sh
oc apply -f apicurio/instance/es-kafka-topics.yaml
oc apply -f apicurio/instance/registry.yaml       
```

For Strimzi as Kafka backend

```sh
oc apply -k apicurio/instance/
```

This should create a registry instance using Kafka as persistence:

* A deployment
* A service with the name of the registry: `eda-registry-service`
* One pod

The pod's logs should display successful connection to Kafka:

```logs
(KSQL Kafka Consumer Thread) Kafka version: 2.5.0
2022-01-09 18:31:58,670 INFO  [org.apa.kaf.com.uti.AppInfoParser] (KSQL Kafka Consumer Thread) Kafka commitId: 66563e712b0b9f84
2022-01-09 18:31:58,670 INFO  [org.apa.kaf.com.uti.AppInfoParser] (KSQL Kafka Consumer Thread) Kafka startTimeMs: 1641753118669
2022-01-09 18:31:58,726 INFO  [org.apa.kaf.cli.con.KafkaConsumer] (KSQL Kafka Consumer Thread) [Consumer clientId=consumer-6956ab24-2139-484a-8942-6c76edad14df-1, groupId=6956ab24-2139-484a-8942-6c76edad14df] Subscribed to topic(s): kafkasql-journal
2022-01-09 18:31:58,751 INFO  [org.apa.kaf.cli.Metadata] (KSQL Kafka Consumer Thread) [Consumer clientId=consumer-6956ab24-2139-484a-8942-6c76edad14df-1, groupId=6956ab24-2139-484a-8942-6c76edad14df] Cluster ID: jHNqiapBTSGtW0YO7w-JMQ
2022-01-09 18:31:58,752 INFO  [org.apa.kaf.cli.con.int.AbstractCoordinator] (KSQL Kafka Consumer Thread) [Consumer clientId=consumer-6956ab24-2139-484a-8942-6c76edad14df-1, groupId=6956ab24-2139-484a-8942-6c76edad14df] Discovered group coordinator dev-kafka-2.dev-kafka-brokers.edademo-dev.svc:9092 (id: 2147483645 rack: null)
2022-01-09 18:31:58,757 INFO  [org.apa.kaf.cli.con.int.AbstractCoordinator] (KSQL Kafka Consumer Thread) [Consumer clientId=consumer-6956ab24-2139-484a-8942-6c76edad14df-1, groupId=6956ab24-2139-484a-8942-6c76edad14df] (Re-)joining group
```

Access to the UI can be done by adding a route:

