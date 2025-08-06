# JMolecules Jackson Integration funktioniert nicht mit Kotlin

## Meta
Status: Nicht reproduzierbar. Testcase hinzugefügt, der nicht fehlschlägt.
Ticket: https://github.com/xmolecules/jmolecules-integrations/issues/336

Bei korrektem Kotlin Jackson Setup (siehe Commit an dem Ticket oben) funktioniert das. Es ist wichtig, das Jackson Kotlin Modul zu registrieren. Boot scheint das aktuell nicht automatisch zu registrieren. Ich habe das ans Team weitergetragen. Ist das aktiv, funktioniert die Instantierung von Single-Value data-Classes die `Identifier` oder `ValueObject` sind sowohl mit statischer Factory-Methode als auch ohne. In zweitem Fall wird einfach der Konstruktor genutzt.

## Zusammenfassung
Wenn ein JMolecules Identifier in einem DTO, [JMolecules-Jackson Integration](https://github.com/xmolecules/jmolecules-integrations/tree/main/jmolecules-jackson) genutzt wird und die Factory richtig implementiert ist, wird die Factory trotzdem von Jackson nicht gefunden.

## Code
```kotlin  
data class LegacyEmployeeChangedEvent(    
	val id: String,    
	val firstName: String,    
	val lastName: String,    
	val email: String,    
	val active: Boolean,    
	val thumbnail: ByteArray?  
)  
```  
  
```kotlin  
@KafkaListener(id = "employee-created-legacy-listener", topics = ["\${legacy-topics.employees.created}"])  
fun listenToEmployeeCreatedEvent(event: LegacyEmployeeChangedEvent) {  
    logger.info{ "Received new employee created event" }  
    val employee = repository.save(Employee(  
        id = event.id,
		firstName = event.firstName,
		lastName = event.lastName,
		email = event.email,
		active = event.active,
		thumbnail = event.thumbnail,
	))  
    logger.info { "Created employee with id ${employee.id}" }  
}  
```

## Ursache
JMolecules Jackson deserialized Werte, indem es [eine statisch Factory-Methode namens `of` sucht](https://github.com/xmolecules/jmolecules-integrations/tree/main/jmolecules-jackson). Die Methode wird [mittels Java Reflection gesucht](https://github.com/xmolecules/jmolecules-integrations/blob/aab271890dc2767e13d3f2bab72a1b3b23e230fe/jmolecules-jackson/src/main/java/org/jmolecules/jackson/SingleValueWrappingTypeDeserializerModifier.java#L101), spezifisch wird dabei folgender Code ausgeführt:

```java
private static Method findFactoryMethodOn(Class<?> type, Class<?> parameterType) {

	try {

		Method method = type.getDeclaredMethod("of", parameterType);
		return Modifier.isStatic(method.getModifiers()) ? method : null;

	} catch (Exception e) {
		return null;
	}
}
```

Das Kotlin Äquivalent davon sind [Companion Objects](https://kotlinlang.org/docs/object-declarations.html#companion-objects). Sie erlauben genauso, wie statische Java Methoden, dass Methoden ohne Instanziierung eines Objektes aufgerufen werden. Companion Objects sind jedoch nicht statisch, die Methoden gehören zum Object. Damit der Compiler statische Methoden erstellt, [muss die Methode mit `@JvmStatic` annotiert werden](https://kotlinlang.org/docs/java-to-kotlin-interop.html#static-methods). Wenn dies gemacht wird wirft `type.getDeclaredMethod("of", parameterType)` trotzdem eine `MethodNotFoundException`. 


## Lösung
In den DTOs den primitiven Datentype des Identifiers nutzen und manuell mappen.

### Code
```kotlin
data class LegacyEmployeeChangedEvent(  
    val id: String,  
    val firstName: String,  
    val lastName: String,  
    val email: String,  
    val active: Boolean,  
    val thumbnail: ByteArray?  
)
```

```kotlin
val employee = repository.save(Employee(  
    id = EmployeeId(event.id),  
    firstName = event.firstName,  
    lastName = event.lastName,  
    email = event.email,  
    active = event.active,  
    thumbnail = event.thumbnail,  
))
```
