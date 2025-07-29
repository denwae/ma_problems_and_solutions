# `@Externalized` Serialization Probleme
Beim Listener kommen Objekte stets im Byteformat an. Dabei ist nicht richtig ersichtlich wieso dies passiert, da Kafka als 
Producer nur in Json serialisieren sollte. Es wird dann nicht richtig deserialized und kafka kann dann nicht die Payload an das objekt mappen. 


## Code
### Stack Trace

```javastacktrace
org.springframework.kafka.listener.ListenerExecutionFailedException: Listener failed
 at org.springframework.kafka.listener.KafkaMessageListenerContainer$ListenerConsumer.decorateException(KafkaMessageListenerContainer.java:2873) ~[spring-kafka-3.2.3.jar:3.2.3]
 at org.springframework.kafka.listener.KafkaMessageListenerContainer$ListenerConsumer.doInvokeOnMessage(KafkaMessageListenerContainer.java:2814) ~[spring-kafka-3.2.3.jar:3.2.3]
 at org.springframework.kafka.listener.KafkaMessageListenerContainer$ListenerConsumer.invokeOnMessage(KafkaMessageListenerContainer.java:2778) ~[spring-kafka-3.2.3.jar:3.2.3]
 at org.springframework.kafka.listener.KafkaMessageListenerContainer$ListenerConsumer.lambda$doInvokeRecordListener$53(KafkaMessageListenerContainer.java:2701) ~[spring-kafka-3.2.3.jar:3.2.3]
 at io.micrometer.observation.Observation.observe(Observation.java:565) ~[micrometer-observation-1.13.2.jar:1.13.2]
 at org.springframework.kafka.listener.KafkaMessageListenerContainer$ListenerConsumer.doInvokeRecordListener(KafkaMessageListenerContainer.java:2699) ~[spring-kafka-3.2.3.jar:3.2.3]
 at org.springframework.kafka.listener.KafkaMessageListenerContainer$ListenerConsumer.doInvokeWithRecords(KafkaMessageListenerContainer.java:2541) ~[spring-kafka-3.2.3.jar:3.2.3]
 at org.springframework.kafka.listener.KafkaMessageListenerContainer$ListenerConsumer.invokeRecordListener(KafkaMessageListenerContainer.java:2430) ~[spring-kafka-3.2.3.jar:3.2.3]
 at org.springframework.kafka.listener.KafkaMessageListenerContainer$ListenerConsumer.invokeListener(KafkaMessageListenerContainer.java:2085) ~[spring-kafka-3.2.3.jar:3.2.3]
 at org.springframework.kafka.listener.KafkaMessageListenerContainer$ListenerConsumer.invokeIfHaveRecords(KafkaMessageListenerContainer.java:1461) ~[spring-kafka-3.2.3.jar:3.2.3]
 at org.springframework.kafka.listener.KafkaMessageListenerContainer$ListenerConsumer.pollAndInvoke(KafkaMessageListenerContainer.java:1426) ~[spring-kafka-3.2.3.jar:3.2.3]
 at org.springframework.kafka.listener.KafkaMessageListenerContainer$ListenerConsumer.run(KafkaMessageListenerContainer.java:1296) ~[spring-kafka-3.2.3.jar:3.2.3]
 at java.base/java.util.concurrent.CompletableFuture$AsyncRun.run(CompletableFuture.java:1804) ~[na:na]
 at java.base/java.lang.Thread.run(Thread.java:840) ~[na:na]
Caused by: org.springframework.kafka.support.converter.ConversionException: Failed to convert from JSON
 at org.springframework.kafka.support.converter.JsonMessageConverter.extractAndConvertValue(JsonMessageConverter.java:124) ~[spring-kafka-3.2.3.jar:3.2.3]
 at org.springframework.kafka.support.converter.MessagingMessageConverter.toMessage(MessagingMessageConverter.java:192) ~[spring-kafka-3.2.3.jar:3.2.3]
 at org.springframework.kafka.listener.adapter.MessagingMessageListenerAdapter.toMessagingMessage(MessagingMessageListenerAdapter.java:377) ~[spring-kafka-3.2.3.jar:3.2.3]
 at org.springframework.kafka.listener.adapter.RecordMessagingMessageListenerAdapter.onMessage(RecordMessagingMessageListenerAdapter.java:77) ~[spring-kafka-3.2.3.jar:3.2.3]
 at org.springframework.kafka.listener.adapter.RecordMessagingMessageListenerAdapter.onMessage(RecordMessagingMessageListenerAdapter.java:50) ~[spring-kafka-3.2.3.jar:3.2.3]
 at org.springframework.kafka.listener.KafkaMessageListenerContainer$ListenerConsumer.doInvokeOnMessage(KafkaMessageListenerContainer.java:2800) ~[spring-kafka-3.2.3.jar:3.2.3]
 ... 12 common frames omitted
Caused by: com.fasterxml.jackson.databind.exc.MismatchedInputException: Cannot construct instance of `de.adesso.referencer.create.service.rest.eventing.TemplateUpdatedEventLegacy` (although at least one Creator exists): no String-argument constructor/factory method to deserialize from String value 
```
## Ursache

**Vermutung**: Spring überschreibt mit internen mappern, wie das Objekt an Kafka gegeben wird. Dabei ist nicht klar, welche dies sind. Es wurde bereits versucht, die Serialisierung von Spring Modulith zu deaktivieren und einen custom serializer zu implementieren, der das event nur an kafka weiterreicht und den standard serializer überschreibt. Beides zeigte keine Veränderung.

# Lösung

Mit dem Kafka Template klappt es. 