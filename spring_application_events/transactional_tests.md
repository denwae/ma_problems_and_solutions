# `@Transactional` Annotation und Spies in Tests

Die Spies erhalten nie ein Event, da standardmäßig ein `@TransactionalEventListener` sein Event erst nach dem Commit der umgebenden Transaktion feuert. Im Test wird die Transaktion durch die `@Transactional` Annotation am Ende aber immer zurückgerollt und damit läuft dein Listener niemals an.

## Code

### Stack Trace
```javastacktrace
templateUpdatedEventListener.handleTemplateUpdatedEvent(
	TemplateUpdatedEvent(companyId=0f7a32c6-290e-442f-b1e9-b2507dbe83c9, contentType=PPTX, fileBytes=[0, 0, 0, 0, 0])
);
-> at de.adesso.referencer.references.internal.TemplateUpdatedEventListener.handleTemplateUpdatedEvent(TemplateUpdatedEventListener.kt:20)
Actually, there were zero interactions with this mock.
within 10 seconds.
	at org.awaitility.core.ConditionAwaiter.await(ConditionAwaiter.java:167)
	at org.awaitility.core.AssertionCondition.await(AssertionCondition.java:119)
	at org.awaitility.core.AssertionCondition.await(AssertionCondition.java:31)
	at org.awaitility.core.ConditionFactory.until(ConditionFactory.java:1006)
	at org.awaitility.core.ConditionFactory.untilAsserted(ConditionFactory.java:790)
	at org.awaitility.kotlin.AwaitilityKt.untilAsserted(AwaitilityKt.kt:156)
	at de.adesso.referencer.integration.company.CompanyManagementTest$Given an existing company in the database$and a valid pptx file$when updating the pptx template.then expect templateUpdatedEventListener to receive an Event(CompanyManagementTest.kt:281)
	at java.base/java.lang.reflect.Method.invoke(Method.java:580)
	at java.base/java.util.ArrayList.forEach(ArrayList.java:1596)
	at java.base/java.util.ArrayList.forEach(ArrayList.java:1596)
	at java.base/java.util.ArrayList.forEach(ArrayList.java:1596)
	at java.base/java.util.ArrayList.forEach(ArrayList.java:1596)
	at java.base/java.util.ArrayList.forEach(ArrayList.java:1596)
Caused by: Wanted but not invoked:
templateUpdatedEventListener.handleTemplateUpdatedEvent(
	TemplateUpdatedEvent(companyId=0f7a32c6-290e-442f-b1e9-b2507dbe83c9, contentType=PPTX, fileBytes=[0, 0, 0, 0, 0])
);
-> at de.adesso.referencer.references.internal.TemplateUpdatedEventListener.handleTemplateUpdatedEvent(TemplateUpdatedEventListener.kt:20)
Actually, there were zero interactions with this mock.
```

## Lösung

`@Transactional` weglassen und manuelle Bereinigung der Datenbankzugriffe.

### Code
```kotlin
@AfterEach
fun cleanUp() {
	companyRepository.deleteAll()
}

```