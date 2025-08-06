# Entities in Value Objects werden nicht richt gemapped

## Meta

Status: In Frage
Fragen: Es ergibt aus DDD-Sicht keinen Sinn, aus ValueObjects Entitäten zu referenzieren, da erstere immutable sind und letztere nicht. Ich vermute die Verirrung kommt daher, dass JPA es erlaubt aus einem Embeddable Entitäten zu referenzieren. Ich würde empfehlen, aus dem ValueObject lediglich eine ID zu referenzieren, üblicherweise auch jediglich eine eines Aggregates, da Entitäten in DDD eigentlich nur lokale Identität haben.

## Zusammenfassung
Wenn eine Entity innerhalb eines Value Objects referenziert wird, wird eine Hibernate `MappingException` geworfen.

## Code
```kotlin
data class ContentData(  
	// ommited for brevity
    val advantages: MutableList<Advantage> = mutableListOf()  
): ValueObject
```

### Stack Trace
```javastacktrace
org.junit.jupiter.api.extension.ParameterResolutionException: Failed to resolve parameter [de.adesso.referencer.references.internal.foreign.employees.ReferenceEmployeeRepository employeeRepository] in constructor [public de.adesso.referencer.integration.references.foreign.employees.EmployeeLegacyListenerTest(de.adesso.referencer.references.internal.foreign.employees.ReferenceEmployeeRepository,org.springframework.kafka.core.KafkaTemplate<java.lang.String, de.adesso.referencer.core.LegacyEmployeesEvent>,org.testcontainers.containers.MariaDBContainer<?>,org.testcontainers.redpanda.RedpandaContainer)]: Failed to load ApplicationContext for [WebMergedContextConfiguration@1611db84 testClass = de.adesso.referencer.integration.references.foreign.employees.EmployeeLegacyListenerTest, locations = [], classes = [de.adesso.referencer.ReferencerApplication], contextInitializerClasses = [], activeProfiles = ["test"], propertySourceDescriptors = [], propertySourceProperties = ["org.springframework.boot.test.context.SpringBootTestContextBootstrapper=true"], contextCustomizers = [[ImportsContextCustomizer@54b85ab key = [de.adesso.referencer.TestcontainersConfiguration, org.springframework.modulith.test.ModuleTestAutoConfiguration]], org.springframework.boot.test.context.filter.ExcludeFilterContextCustomizer@b692323, org.springframework.boot.test.json.DuplicateJsonObjectContextCustomizerFactory$DuplicateJsonObjectContextCustomizer@22f3a5bc, org.springframework.boot.test.mock.mockito.MockitoContextCustomizer@0, org.springframework.boot.test.web.client.TestRestTemplateContextCustomizer@6dafe488, org.springframework.boot.test.web.reactor.netty.DisableReactorResourceFactoryGlobalResourcesContextCustomizerFactory$DisableReactorResourceFactoryGlobalResourcesContextCustomizerCustomizer@8936d23, org.springframework.boot.test.autoconfigure.OnFailureConditionReportContextCustomizerFactory$OnFailureConditionReportContextCustomizer@43f13160, org.springframework.boot.test.autoconfigure.actuate.observability.ObservabilityContextCustomizerFactory$DisableObservabilityContextCustomizer@1f, org.springframework.boot.test.autoconfigure.filter.TypeExcludeFiltersContextCustomizer@3c419650, org.springframework.boot.test.autoconfigure.properties.PropertyMappingContextCustomizer@0, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverContextCustomizer@7f7d5133, org.springframework.test.context.support.DynamicPropertiesContextCustomizer@0, org.springframework.boot.testcontainers.service.connection.ServiceConnectionContextCustomizer@0, org.springframework.modulith.test.ModuleContextCustomizerFactory$ModuleContextCustomizer@3947e168, org.springframework.boot.test.context.SpringBootTestAnnotation@e844d32d], resourceBasePath = "src/main/webapp", contextLoader = org.springframework.boot.test.context.SpringBootContextLoader, parent = null]

	at java.base/java.util.Optional.orElseGet(Optional.java:364)
	at java.base/java.util.ArrayList.forEach(ArrayList.java:1596)
Caused by: java.lang.IllegalStateException: Failed to load ApplicationContext for [WebMergedContextConfiguration@1611db84 testClass = de.adesso.referencer.integration.references.foreign.employees.EmployeeLegacyListenerTest, locations = [], classes = [de.adesso.referencer.ReferencerApplication], contextInitializerClasses = [], activeProfiles = ["test"], propertySourceDescriptors = [], propertySourceProperties = ["org.springframework.boot.test.context.SpringBootTestContextBootstrapper=true"], contextCustomizers = [[ImportsContextCustomizer@54b85ab key = [de.adesso.referencer.TestcontainersConfiguration, org.springframework.modulith.test.ModuleTestAutoConfiguration]], org.springframework.boot.test.context.filter.ExcludeFilterContextCustomizer@b692323, org.springframework.boot.test.json.DuplicateJsonObjectContextCustomizerFactory$DuplicateJsonObjectContextCustomizer@22f3a5bc, org.springframework.boot.test.mock.mockito.MockitoContextCustomizer@0, org.springframework.boot.test.web.client.TestRestTemplateContextCustomizer@6dafe488, org.springframework.boot.test.web.reactor.netty.DisableReactorResourceFactoryGlobalResourcesContextCustomizerFactory$DisableReactorResourceFactoryGlobalResourcesContextCustomizerCustomizer@8936d23, org.springframework.boot.test.autoconfigure.OnFailureConditionReportContextCustomizerFactory$OnFailureConditionReportContextCustomizer@43f13160, org.springframework.boot.test.autoconfigure.actuate.observability.ObservabilityContextCustomizerFactory$DisableObservabilityContextCustomizer@1f, org.springframework.boot.test.autoconfigure.filter.TypeExcludeFiltersContextCustomizer@3c419650, org.springframework.boot.test.autoconfigure.properties.PropertyMappingContextCustomizer@0, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverContextCustomizer@7f7d5133, org.springframework.test.context.support.DynamicPropertiesContextCustomizer@0, org.springframework.boot.testcontainers.service.connection.ServiceConnectionContextCustomizer@0, org.springframework.modulith.test.ModuleContextCustomizerFactory$ModuleContextCustomizer@3947e168, org.springframework.boot.test.context.SpringBootTestAnnotation@e844d32d], resourceBasePath = "src/main/webapp", contextLoader = org.springframework.boot.test.context.SpringBootContextLoader, parent = null]
	at org.springframework.test.context.cache.DefaultCacheAwareContextLoaderDelegate.loadContext(DefaultCacheAwareContextLoaderDelegate.java:180)
	at org.springframework.test.context.support.DefaultTestContext.getApplicationContext(DefaultTestContext.java:130)
	at org.springframework.test.context.junit.jupiter.SpringExtension.getApplicationContext(SpringExtension.java:352)
	at org.springframework.test.context.junit.jupiter.SpringExtension.resolveParameter(SpringExtension.java:338)
	... 2 more
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: Basic collection has element type 'de.adesso.referencer.references.internal.Advantage' which is not a known basic type (attribute is not annotated '@ElementCollection', '@OneToMany', or '@ManyToMany')
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
	... 5 more
Caused by: org.hibernate.MappingException: Basic collection has element type 'de.adesso.referencer.references.internal.Advantage' which is not a known basic type (attribute is not annotated '@ElementCollection', '@OneToMany', or '@ManyToMany')
	at org.hibernate.type.descriptor.java.spi.BasicCollectionJavaType.getRecommendedJdbcType(BasicCollectionJavaType.java:72)
	at org.hibernate.boot.model.process.internal.InferredBasicValueResolver.from(InferredBasicValueResolver.java:195)
	at org.hibernate.mapping.BasicValue.resolution(BasicValue.java:664)
	at org.hibernate.mapping.BasicValue.buildResolution(BasicValue.java:461)
	at org.hibernate.mapping.BasicValue.resolve(BasicValue.java:351)
	at org.hibernate.mapping.BasicValue.getType(BasicValue.java:329)
	at org.hibernate.type.ComponentType.<init>(ComponentType.java:104)
	at org.hibernate.type.ComponentType.<init>(ComponentType.java:82)
	at org.hibernate.mapping.Component.getType(Component.java:435)
	at org.hibernate.mapping.Component.getType(Component.java:79)
	at org.hibernate.mapping.Property.getType(Property.java:87)
	at org.hibernate.jpa.event.internal.CallbackDefinitionResolverLegacyImpl.resolveEmbeddableCallbacks(CallbackDefinitionResolverLegacyImpl.java:176)
	at org.hibernate.boot.model.internal.EntityBinder.lambda$bindCallbacks$edfcf893$1(EntityBinder.java:1229)
	at org.hibernate.boot.internal.InFlightMetadataCollectorImpl.processSecondPasses(InFlightMetadataCollectorImpl.java:1842)
	at org.hibernate.boot.internal.InFlightMetadataCollectorImpl.processSecondPasses(InFlightMetadataCollectorImpl.java:1800)
	at org.hibernate.boot.model.process.spi.MetadataBuildingProcess.complete(MetadataBuildingProcess.java:334)
	at org.hibernate.jpa.boot.internal.EntityManagerFactoryBuilderImpl.metadata(EntityManagerFactoryBuilderImpl.java:1442)
	at org.hibernate.jpa.boot.internal.EntityManagerFactoryBuilderImpl.build(EntityManagerFactoryBuilderImpl.java:1513)
	at org.springframework.orm.jpa.vendor.SpringHibernateJpaPersistenceProvider.createContainerEntityManagerFactory(SpringHibernateJpaPersistenceProvider.java:66)
	at org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean.createNativeEntityManagerFactory(LocalContainerEntityManagerFactoryBean.java:390)
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.buildNativeEntityManagerFactory(AbstractEntityManagerFactoryBean.java:419)
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.afterPropertiesSet(AbstractEntityManagerFactoryBean.java:400)
	at org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean.afterPropertiesSet(LocalContainerEntityManagerFactoryBean.java:366)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.invokeInitMethods(AbstractAutowireCapableBeanFactory.java:1859)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1808)
	... 25 more
```

## Ursache
Die JMolecules ByteBuddy Plugin annotiert Entities innerhalb von Value Objects nicht.

## Lösung
### Ansätze
- Die Entity manuell referenzieren
- Entity in Aggregate Root auslagern
- Das Value Object in eine Entity umwandeln

### Gewählte Lösung
Die Entity in die Aggregate Root auslagern. 

## Code
```kotlin
class Reference(  
    override val id: ReferenceId = ReferenceId(),  
    var administrativeData: AdministrativeData,  
    var metadata: ReferenceMetadata,  
    ) : AggregateRoot<Reference, ReferenceId> {  

    // ommited for brevity
    var contentData: ContentData = ContentData(freeTextTemplateName = "TODO: calculate later")  
}
```