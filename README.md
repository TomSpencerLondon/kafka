### Setup
### Download Kafka here:
https://kafka.apache.org/downloads
### Add servers
- kafka/kraft-setup-in-kafka/kraft/server-1.properties
- kafka/kraft-setup-in-kafka/kraft/server-2.properties
- kafka/kraft-setup-in-kafka/kraft/server-3.properties

### Run servers
```
kafka_2.13-3.9.0 ./bin/kafka-server-start.sh config/kraft/server-1.properties
kafka_2.13-3.9.0 ./bin/kafka-server-start.sh config/kraft/server-2.properties
kafka_2.13-3.9.0 ./bin/kafka-server-start.sh config/kraft/server-3.properties
```

### Run consumer on cli just for fun
```bash
➜  kafka_2.13-3.9.0  ./bin/kafka-console-consumer.sh --topic product-created-events-topic --bootstrap-server localhost:9092 --property print.key=true
53548157-5381-4fe7-9090-03d1fc27ca57	{"productId":"53548157-5381-4fe7-9090-03d1fc27ca57","title":"iPhone 11","price":800,"quantity":19}
7f9f77f0-a33f-466e-a166-074fd9d6e02e	{"productId":"7f9f77f0-a33f-466e-a166-074fd9d6e02e","title":"iPhone 11","price":800,"quantity":19}
```
We already have the emailNotification as the consumer service so we can check the logs there.

### Products Application
```bash
2024-11-09T17:31:28.974Z  INFO 96569 --- [o-auto-1-exec-2] o.a.k.clients.producer.KafkaProducer     : [Producer clientId=producer-1] Instantiated an idempotent producer.
2024-11-09T17:31:28.991Z  INFO 96569 --- [o-auto-1-exec-2] o.a.kafka.common.utils.AppInfoParser     : Kafka version: 3.6.0
2024-11-09T17:31:28.991Z  INFO 96569 --- [o-auto-1-exec-2] o.a.kafka.common.utils.AppInfoParser     : Kafka commitId: 60e845626d8a465a
2024-11-09T17:31:28.991Z  INFO 96569 --- [o-auto-1-exec-2] o.a.kafka.common.utils.AppInfoParser     : Kafka startTimeMs: 1731173488990
2024-11-09T17:31:29.011Z  INFO 96569 --- [ad | producer-1] org.apache.kafka.clients.Metadata        : [Producer clientId=producer-1] Cluster ID: h0hwSwWLS_y17GpFz8WJaA
2024-11-09T17:31:29.259Z  INFO 96569 --- [ad | producer-1] o.a.k.c.p.internals.TransactionManager   : [Producer clientId=producer-1] ProducerId set to 2000 with epoch 0
2024-11-09T17:31:29.345Z  INFO 96569 --- [o-auto-1-exec-2] org.example.service.ProductServiceImpl   : Partition: 0
2024-11-09T17:31:29.345Z  INFO 96569 --- [o-auto-1-exec-2] org.example.service.ProductServiceImpl   : Topic: product-created-events-topic
2024-11-09T17:31:29.345Z  INFO 96569 --- [o-auto-1-exec-2] org.example.service.ProductServiceImpl   : Offset: 0
2024-11-09T17:31:29.345Z  INFO 96569 --- [o-auto-1-exec-2] org.example.service.ProductServiceImpl   : ***** Returning product id
2024-11-09T17:34:48.128Z  INFO 96569 --- [o-auto-1-exec-4] org.example.service.ProductServiceImpl   : Before publishing a ProductCreatedEvent
2024-11-09T17:34:48.188Z  INFO 96569 --- [o-auto-1-exec-4] org.example.service.ProductServiceImpl   : Partition: 0
2024-11-09T17:34:48.188Z  INFO 96569 --- [o-auto-1-exec-4] org.example.service.ProductServiceImpl   : Topic: product-created-events-topic
2024-11-09T17:34:48.188Z  INFO 96569 --- [o-auto-1-exec-4] org.example.service.ProductServiceImpl   : Offset: 1
2024-11-09T17:34:48.188Z  INFO 96569 --- [o-auto-1-exec-4] org.example.service.ProductServiceImpl   : ***** Returning product id
```

### Email Notification Application
```bash
2024-11-09T17:34:45.801Z  INFO 98952 --- [ntainer#0-0-C-1] o.e.handler.ProductCreatedEventHandler   : Received a new event: iPhone 11
2024-11-09T17:34:48.188Z  INFO 98952 --- [ntainer#0-0-C-1] o.e.handler.ProductCreatedEventHandler   : Received a new event: iPhone 11
```

### Postman request
<img width="1302" alt="image" src="https://github.com/user-attachments/assets/1014da4c-f351-48be-a9d0-94087c904ed8">


This video was quite good for creating the multi module application:
https://www.youtube.com/watch?v=XqC1zeFdxMs