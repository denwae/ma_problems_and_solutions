# ApplicationEvents können nicht mit Konstruktor-Injektion injiziert werden

In Spring-Tests ist ApplicationEvents kein reguläres Bean im ApplicationContext, sondern ein Test-Helper, der nach der Test-Instanzierung vom TestContext-Framework eingespeist wird.

## Code
```kotlin
@Autowired
constructor(
    private val companyRepository: CompanyRepository,
    private val mockMvc: MockMvc,
    @MockitoSpyBean
    private var templateUpdatedEventListener :TemplateUpdatedEventListener,
    private val events: ApplicationEvents,
    private val redpandaContainer: RedpandaContainer
) :AbstractIntegrationTest(redpandaContainer)
```

### Stack Trace
```javastacktrace
Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'org.springframework.test.context.event.ApplicationEvents' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {}
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.raiseNoMatchingBeanFound(DefaultListableBeanFactory.java:2207)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1630)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1555)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.resolveDependency(AbstractAutowireCapableBeanFactory.java:472)
	at org.springframework.beans.factory.annotation.ParameterResolutionDelegate.resolveDependency(ParameterResolutionDelegate.java:136)
	at org.springframework.test.context.junit.jupiter.SpringExtension.resolveParameter(SpringExtension.java:339)
... 9 more
```

## Ursache

ApplicationEvents ist keine Spring-Bean.
Sie wird vom EventPublishingTestExecutionListener des TestContext erzeugt 
und nachträglich in die Test-Instanz geschrieben,
nicht über den normalen ApplicationContext

## Lösung

Verwende für ApplicationEvents in Tests die
Feld-Injektion.

### Code
```kotlin
class CompanyManagementTest
@Autowired
constructor(
    private val companyRepository: CompanyRepository,
    private val mockMvc: MockMvc,
    @MockitoSpyBean private var templateUpdatedEventListener :TemplateUpdatedEventListener,
    private val redpandaContainer: RedpandaContainer
) :AbstractIntegrationTest(redpandaContainer) {

    @Autowired
    private lateinit var events: ApplicationEvents
}
```