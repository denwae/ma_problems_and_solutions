# Es können nicht mehrere ValueObjects desselben Typen in einer Klasse genutzt werden
Hibernate mapped nicht ValueObjects der selben Klasse standardmäßig zu unterschiedlichen Spalten.

## Code
```kotlin
class Reference(/*skipped for brevity*/): AggregateRoot<Reference, ReferenceId> {

	/*skipped for brevity*/
	val projectTimeFrame: TimeFrame? = null  
	val maintenanceTimeFrame: TimeFrame? = null
}
```

```kotlin
class TimeFrame(  
    val start: LocalDate? = null,  
    val end: LocalDate? = null,  
): ValueObject
```

### Stack Trace
```javastacktrace
java.lang.IllegalStateException: Failed to load ApplicationContext for [WebMergedContextConfiguration@154f7867 testClass = de.adesso.referencer.ReferencerApplicationTests, locations = [], classes = [de.adesso.referencer.ReferencerApplication], contextInitializerClasses = [], activeProfiles = [], propertySourceDescriptors = [], propertySourceProperties = ["org.springframework.boot.test.context.SpringBootTestContextBootstrapper=true"], contextCustomizers = [[ImportsContextCustomizer@45519e74 key = [de.adesso.referencer.TestcontainersConfiguration]], org.springframework.boot.test.context.filter.ExcludeFilterContextCustomizer@2ba9ed19, org.springframework.boot.test.json.DuplicateJsonObjectContextCustomizerFactory$DuplicateJsonObjectContextCustomizer@4e3d36c2, org.springframework.boot.test.mock.mockito.MockitoContextCustomizer@0, org.springframework.boot.test.web.client.TestRestTemplateContextCustomizer@299dd381, org.springframework.boot.test.web.reactor.netty.DisableReactorResourceFactoryGlobalResourcesContextCustomizerFactory$DisableReactorResourceFactoryGlobalResourcesContextCustomizerCustomizer@5d01a2eb, org.springframework.boot.test.autoconfigure.OnFailureConditionReportContextCustomizerFactory$OnFailureConditionReportContextCustomizer@43312512, org.springframework.boot.test.autoconfigure.actuate.observability.ObservabilityContextCustomizerFactory$DisableObservabilityContextCustomizer@1f, org.springframework.boot.test.autoconfigure.properties.PropertyMappingContextCustomizer@0, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverContextCustomizer@445f6f73, org.springframework.test.context.support.DynamicPropertiesContextCustomizer@0, org.springframework.boot.testcontainers.service.connection.ServiceConnectionContextCustomizer@0, org.springframework.boot.test.context.SpringBootTestAnnotation@e463f0bc], resourceBasePath = "src/main/webapp", contextLoader = org.springframework.boot.test.context.SpringBootContextLoader, parent = null]

	at org.springframework.test.context.cache.DefaultCacheAwareContextLoaderDelegate.loadContext(DefaultCacheAwareContextLoaderDelegate.java:180)
	at org.springframework.test.context.support.DefaultTestContext.getApplicationContext(DefaultTestContext.java:130)
	at org.springframework.test.context.web.ServletTestExecutionListener.setUpRequestContextIfNecessary(ServletTestExecutionListener.java:200)
	at org.springframework.test.context.web.ServletTestExecutionListener.prepareTestInstance(ServletTestExecutionListener.java:139)
	at org.springframework.test.context.TestContextManager.prepareTestInstance(TestContextManager.java:260)
	at org.springframework.test.context.junit.jupiter.SpringExtension.postProcessTestInstance(SpringExtension.java:160)
	at java.base/java.util.stream.ForEachOps$ForEachOp$OfRef.accept(ForEachOps.java:184)
	at java.base/java.util.stream.ReferencePipeline$3$1.accept(ReferencePipeline.java:197)
	at java.base/java.util.stream.ReferencePipeline$2$1.accept(ReferencePipeline.java:179)
	at java.base/java.util.stream.ReferencePipeline$3$1.accept(ReferencePipeline.java:197)
	at java.base/java.util.ArrayList$ArrayListSpliterator.forEachRemaining(ArrayList.java:1708)
	at java.base/java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:509)
	at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:499)
	at java.base/java.util.stream.ForEachOps$ForEachOp.evaluateSequential(ForEachOps.java:151)
	at java.base/java.util.stream.ForEachOps$ForEachOp$OfRef.evaluateSequential(ForEachOps.java:174)
	at java.base/java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:234)
	at java.base/java.util.stream.ReferencePipeline.forEach(ReferencePipeline.java:596)
	at java.base/java.util.Optional.orElseGet(Optional.java:364)
	at java.base/java.util.ArrayList.forEach(ArrayList.java:1596)
	at java.base/java.util.ArrayList.forEach(ArrayList.java:1596)
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is org.hibernate.MappingException: Column 'end' is duplicated in mapping for entity 'de.adesso.referencer.references.internal.Reference' (use '@Column(insertable=false, updatable=false)' when mapping multiple properties to the same column)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1812)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:601)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:523)
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:339)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:347)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:337)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:207)
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:970)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:627)
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:752)
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:439)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:318)
	at org.springframework.boot.test.context.SpringBootContextLoader.lambda$loadContext$3(SpringBootContextLoader.java:144)
	at org.springframework.util.function.ThrowingSupplier.get(ThrowingSupplier.java:58)
	at org.springframework.util.function.ThrowingSupplier.get(ThrowingSupplier.java:46)
	at org.springframework.boot.SpringApplication.withHook(SpringApplication.java:1461)
	at org.springframework.boot.test.context.SpringBootContextLoader$ContextLoaderHook.run(SpringBootContextLoader.java:563)
	at org.springframework.boot.test.context.SpringBootContextLoader.loadContext(SpringBootContextLoader.java:144)
	at org.springframework.boot.test.context.SpringBootContextLoader.loadContext(SpringBootContextLoader.java:110)
	at org.springframework.test.context.cache.DefaultCacheAwareContextLoaderDelegate.loadContextInternal(DefaultCacheAwareContextLoaderDelegate.java:225)
	at org.springframework.test.context.cache.DefaultCacheAwareContextLoaderDelegate.loadContext(DefaultCacheAwareContextLoaderDelegate.java:152)
	... 19 more
Caused by: jakarta.persistence.PersistenceException: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is org.hibernate.MappingException: Column 'end' is duplicated in mapping for entity 'de.adesso.referencer.references.internal.Reference' (use '@Column(insertable=false, updatable=false)' when mapping multiple properties to the same column)
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.buildNativeEntityManagerFactory(AbstractEntityManagerFactoryBean.java:431)
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.afterPropertiesSet(AbstractEntityManagerFactoryBean.java:400)
	at org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean.afterPropertiesSet(LocalContainerEntityManagerFactoryBean.java:366)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.invokeInitMethods(AbstractAutowireCapableBeanFactory.java:1859)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1808)
	... 39 more
Caused by: org.hibernate.MappingException: Column 'end' is duplicated in mapping for entity 'de.adesso.referencer.references.internal.Reference' (use '@Column(insertable=false, updatable=false)' when mapping multiple properties to the same column)
	at org.hibernate.mapping.Value.checkColumnDuplication(Value.java:196)
	at org.hibernate.mapping.MappingHelper.checkPropertyColumnDuplication(MappingHelper.java:251)
	at org.hibernate.mapping.Component.checkColumnDuplication(Component.java:337)
	at org.hibernate.mapping.MappingHelper.checkPropertyColumnDuplication(MappingHelper.java:251)
	at org.hibernate.mapping.PersistentClass.checkColumnDuplication(PersistentClass.java:941)
	at org.hibernate.mapping.PersistentClass.validate(PersistentClass.java:696)
	at org.hibernate.mapping.RootClass.validate(RootClass.java:286)
	at org.hibernate.boot.internal.MetadataImpl.validate(MetadataImpl.java:505)
	at org.hibernate.internal.SessionFactoryImpl.<init>(SessionFactoryImpl.java:282)
	at org.hibernate.boot.internal.SessionFactoryBuilderImpl.build(SessionFactoryBuilderImpl.java:463)
	at org.hibernate.jpa.boot.internal.EntityManagerFactoryBuilderImpl.build(EntityManagerFactoryBuilderImpl.java:1517)
	at org.springframework.orm.jpa.vendor.SpringHibernateJpaPersistenceProvider.createContainerEntityManagerFactory(SpringHibernateJpaPersistenceProvider.java:66)
	at org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean.createNativeEntityManagerFactory(LocalContainerEntityManagerFactoryBean.java:390)
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.buildNativeEntityManagerFactory(AbstractEntityManagerFactoryBean.java:419)
	... 43 more
```
## Ursache
Hibernate nutzt eine Implicit und Physical [Naming Strategies](https://docs.jboss.org/hibernate/orm/6.6/introduction/html_single/Hibernate_Introduction.html#naming-strategies), um die Namen von Tabellen und Spalten zu nutzen. Spring benutzt standardmäßig eine Implicit [Strategie](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/orm/jpa/hibernate/SpringImplicitNamingStrategy.html), die nahezu identisch mit [Hibernates Standardstrategie](https://docs.jboss.org/hibernate/orm/6.6/javadocs/org/hibernate/boot/model/naming/ImplicitNamingStrategyJpaCompliantImpl.html) ist. Die Änderung an Springs Strategie ist für diesen Use Case unwichtig. Diese Implementierungen nutzen nur den Namen der Leaf Property. Daher wird für die `TimeFrame` Klasse immer `end` und `start` für die unterschiedlichen TimeFrames genutzt.

## Lösung
### Ansätze
- Eine andere Naming Strategy benutzen
	- Vorgefertigte (`ImplicitNamingStrategyComponentPathImpl`)
	- Eigene implementieren
- `@AttributeOverride` Annotation nutzen

### Genutzter Ansatz
Die `@AttributeOverride` Annotation benutzen. Die geht auch in ValueObjects innerhalb von ValueObjects
Die vorgefertigte `ImplicitNamingStrategyComponentPathImpl` sollte eigentlich für den Use Case passen, hat aber trotzdem denselben Fehler, wie oben geworfen. Eine eigene Implementierung wäre längerfristig wahrscheinlich, die bessere Idee, wurde aber aus Zeitgründen nicht implementiert.
## Code

```kotlin
class Reference(/*skipped for brevity*/): AggregateRoot<Reference, ReferenceId> {

	/*skipped for brevity*/
	@AttributeOverrides(  
	    AttributeOverride(name = "start", column = Column(name = "project_start_date")),  
	    AttributeOverride(name = "end", column = Column(name = "project_end_date"))  
	)
	val projectTimeFrame: TimeFrame? = null
	@AttributeOverrides(  
	    AttributeOverride(name = "start", column = Column(name = "maintenance_start_date")),  
	    AttributeOverride(name = "end", column = Column(name = "maintenance_end_date"))  
	)
	val maintenanceTimeFrame: TimeFrame? = null
}
```

```kotlin
class ProjectTimeFrames(  
    @AttributeOverrides(  
        AttributeOverride(name = "start", column = Column(name = "project_start_date")),  
        AttributeOverride(name = "end", column = Column(name = "project_end_date"))  
    )  
    val project: TimeFrame = TimeFrame(),  
    @AttributeOverrides(  
        AttributeOverride(name = "start", column = Column(name = "maintenance_start_date")),  
        AttributeOverride(name = "end", column = Column(name = "maintenance_end_date"))  
    )  
    val maintenance: TimeFrame = TimeFrame()  
): ValueObject
```