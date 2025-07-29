# ApplicationEventPublisher ohne `@Transactional`

Während der Architektur-Migration wurden Events über Spring Application Events verschickt, jedoch konnten Listener, die mit der Annotation `@ApplicationModuleListener` versehen sind, die Events nicht empfangen oder verarbeiteten sie nicht korrekt. Das Problem lag darin, dass die Event-Publisher nicht mit einer Transaktion umgeben waren, wodurch die Events vorzeitig oder gar nicht ausgelöst wurden.

## Ursache

Die Event Publisher-Methode war nicht mit der `@Transactional` Annotation versehen. Dadurch wurden die Events zwar publiziert, aber aufgrund des fehlenden Transaktionskontexts konnten die Listener die Events nicht zuverlässig empfangen und verarbeiten. Insbesondere bei komplexen Architekturmodulen und beim Einsatz von jMolecules ist es wichtig, dass Event-Handler innerhalb einer Transaktion ausgeführt werden, um Konsistenz und zuverlässige Event-Verarbeitung zu gewährleisten.

## Lösung

Die Lösung bestand darin, die Methoden mit der Annotation `@DomainEventPublisher` zusätzlich mit `@Transactional` zu versehen. Dadurch wird sichergestellt, dass die Event-Verarbeitung in einem laufenden Transaktionskontext stattfindet und Events korrekt empfangen und verarbeitet werden.

### Beispiel Code

```kotlin
@Service
class SomeService {

    @DomainEventPublisher
    @Transactional
    fun publishSomeEvent(event: SomeEvent) {
        // Event-Verarbeitung, die eine Transaktion benötigt
    }
}
```