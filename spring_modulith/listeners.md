# Kafka Listener werden bootstrapped trotz `BootstrapMode.STANDALONE`

## Meta
Status: akzeptiert, in Framework 7.0 behoben
Details: Teilen sich Integrationstests einen Messagebroker, so landen Messages, die ein `@AMT` für ein spezielles Modul publiziert in diesem Broker. ApplicationContexte, die im Cache liegen enthalten potentiell Listener, die auf diese Events hören und führen Geschäftslogik aus, die dann den Test, der eigentlich gegen einen anderen ApplicationContext läuft beeinflussen. In Spring Framework 7.0 werden die Kontexte, die gerade nicht benutzt werden "stillgelegt". D. h. sie bekommen explizit nach Nutzung ein `Lifecycle.stop()`-Signal, so dass die Listener abgeschaltet werden, solange der assoziierte `AC` nicht in Benutzung ist. Details [hier](https://github.com/spring-projects/spring-framework/wiki/Spring-Framework-7.0-Release-Notes#pausing-of-test-application-contexts).

## Zusammenfassung
Tests schlagen fehl, weil in vorherigen Tests Daten in die Repositories geschrieben wurden.

## Code
```kotlin
@ApplicationModuleTest(module = "employees", mode = ApplicationModuleTest.BootstrapMode.STANDALONE)  
class EmployeeLegacyListenerTest(  
    val repository: EmployeeRepository,  
    val employeeTopic: EmployeeLegacyTopic,  
    val kafkaTemplateWrite: KafkaTemplate<String, LegacyEmployeeChangedEvent>,  
    val kafkaTemplateDelete: KafkaTemplate<String, LegacyEmployeeDeletedEvent>,  
    redpandaContainer: RedpandaContainer  
) : AbstractIntegrationTest(redpandaContainer) {
	// ommited for brevity
}
```

### Log
```logs
2025-04-24T16:22:20.102+02:00  INFO 19384 --- [referencer] [-listener-0-C-1] [                                                 ] d.i.f.DataCreationEmployeeLegacyListener : Received new employee created event
2025-04-24T16:22:20.103+02:00  INFO 19384 --- [referencer] [-listener-0-C-1] [                                                 ] .i.f.e.ReleasedRefLegacyEmployeeListener : Received new employee created event
2025-04-24T16:22:20.103+02:00  INFO 19384 --- [referencer] [-listener-0-C-1] [                                                 ] .r.i.f.e.ReferenceEmployeeLegacyListener : Received new employee created event
```
## Ursache
Die `KafkaEventListener` aus den anderen Modulen werden, trotz des `BootstraMode.STANDALONE` gebootstrapped, horchen auf die Events und persistieren die neuen Entities.
Dieser [Quelle](https://stackoverflow.com/questions/79057132/issue-with-feign-clients-in-spring-modulith-integration-tests-using-application) nach zu urteilen, werden durch Spring Modulith alle Beans geladen, welche durch Annotations an der Main Klasse aktiviert werden.  [Spring Kafka wird autokonfiguriert](https://docs.spring.io/spring-boot/reference/messaging/kafka.html#messaging.kafka). Es wäre möglich die Autokonfiguration zu entfernen, aber die  `@Modulith` Annotation bietet keine `exclude` an.

## Lösung
Die Repositories in den anderen Testklassen vor den Tests leeren.

## Code
```kotlin
@ApplicationModuleTest(module = "releasedReferences")  
@Transactional  
class ReleasedRefEmployeesRepositoryTest(  
    val repository: ReleasedRefEmployeeRepository,  
    redpandaContainer: RedpandaContainer  
): AbstractIntegrationTest(redpandaContainer) {  
  
    init {  
        companionRepository = repository  
    }  
  
    companion object {  
  
        lateinit var companionRepository: ReleasedRefEmployeeRepository  
  
        @JvmStatic  
        @BeforeAll        fun emptyRepository() {  
            companionRepository.deleteAll()  
        }  
    }
	//ommited for brevity
}
```