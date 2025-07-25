# jMolecules kann nicht Listen von Associations zu JPA mappen
Wenn eine Liste mit `Association` persistiert werden muss wird eine `org.hibernate.MappingException` geworfen, da weder `@ElementCollection`, `@OneToMany` oder `@ManyToMany` genutzt wird.  Wenn eine toMany Annotation genutzt wird, wird ein Fehler geworfen, dass `Association` keine Entity ist. Wenn `@ElementCollection` genutzt wird, wird ein Fehler geworfen, dass der empfohlene Typ für eine `Association` nicht gefunden werden kann.

## Code
```kotlin
class Reference(/*ommited for brevity*/): AggregateRoot<Reference, ReferenceId> {

	/*ommited for brevity*/

	val additionallyContactedEmployees: MutableList<Association<ReferenceEmployee, EmployeeId>> = mutableListOf()
}
```

### Stack Trace
```javastacktrace
java.lang.IllegalStateException: Failed to load ApplicationContext for [WebMergedContextConfiguration@3c619d64 testClass = de.adesso.referencer.ReferencerApplicationTests, locations = [], classes = [de.adesso.referencer.ReferencerApplication], contextInitializerClasses = [], activeProfiles = [], propertySourceDescriptors = [], propertySourceProperties = ["org.springframework.boot.test.context.SpringBootTestContextBootstrapper=true"], contextCustomizers = [[ImportsContextCustomizer@3b78110b key = [de.adesso.referencer.TestcontainersConfiguration]], org.springframework.boot.test.context.filter.ExcludeFilterContextCustomizer@37e491e2, org.springframework.boot.test.json.DuplicateJsonObjectContextCustomizerFactory$DuplicateJsonObjectContextCustomizer@5bad555b, org.springframework.boot.test.mock.mockito.MockitoContextCustomizer@0, org.springframework.boot.test.web.client.TestRestTemplateContextCustomizer@7cecab19, org.springframework.boot.test.web.reactor.netty.DisableReactorResourceFactoryGlobalResourcesContextCustomizerFactory$DisableReactorResourceFactoryGlobalResourcesContextCustomizerCustomizer@a7ae340, org.springframework.boot.test.autoconfigure.OnFailureConditionReportContextCustomizerFactory$OnFailureConditionReportContextCustomizer@59a2bed1, org.springframework.boot.test.autoconfigure.actuate.observability.ObservabilityContextCustomizerFactory$DisableObservabilityContextCustomizer@1f, org.springframework.boot.test.autoconfigure.properties.PropertyMappingContextCustomizer@0, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverContextCustomizer@540212be, org.springframework.test.context.support.DynamicPropertiesContextCustomizer@0, org.springframework.boot.testcontainers.service.connection.ServiceConnectionContextCustomizer@0, org.springframework.boot.test.context.SpringBootTestAnnotation@1443c6be], resourceBasePath = "src/main/webapp", contextLoader = org.springframework.boot.test.context.SpringBootContextLoader, parent = null]

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
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: Basic collection has element type 'org.jmolecules.ddd.types.Association' which is not a known basic type (attribute is not annotated '@ElementCollection', '@OneToMany', or '@ManyToMany')
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
Caused by: org.hibernate.MappingException: Basic collection has element type 'org.jmolecules.ddd.types.Association' which is not a known basic type (attribute is not annotated '@ElementCollection', '@OneToMany', or '@ManyToMany')
	at org.hibernate.type.descriptor.java.spi.BasicCollectionJavaType.getRecommendedJdbcType(BasicCollectionJavaType.java:72)
	at org.hibernate.boot.model.process.internal.InferredBasicValueResolver.from(InferredBasicValueResolver.java:195)
	at org.hibernate.mapping.BasicValue.resolution(BasicValue.java:664)
	at org.hibernate.mapping.BasicValue.buildResolution(BasicValue.java:461)
	at org.hibernate.mapping.BasicValue.resolve(BasicValue.java:351)
	at org.hibernate.mapping.BasicValue.resolve(BasicValue.java:341)
	at org.hibernate.boot.internal.InFlightMetadataCollectorImpl.lambda$processValueResolvers$6(InFlightMetadataCollectorImpl.java:1827)
	at java.base/java.util.ArrayList.removeIf(ArrayList.java:1765)
	at java.base/java.util.ArrayList.removeIf(ArrayList.java:1743)
	at org.hibernate.boot.internal.InFlightMetadataCollectorImpl.processValueResolvers(InFlightMetadataCollectorImpl.java:1826)
	at org.hibernate.boot.internal.InFlightMetadataCollectorImpl.processSecondPasses(InFlightMetadataCollectorImpl.java:1812)
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
	... 39 more
```

## Lösung
Einen JPA [`AttributeConverter`](https://jakarta.ee/specifications/persistence/2.2/apidocs/javax/persistence/attributeconverter) implementieren und `@ElementCollection nutzen`.

## Code
```kotlin
class Reference(/*ommited for brevity*/): AggregateRoot<Reference, ReferenceId> {

	/*ommited for brevity*/

	@ElementCollection  
	@Convert(converter = ReferenceEmployeeAssociationConverter::class)  
	val additionallyContactedEmployees: MutableList<Association<ReferenceEmployee, EmployeeId>> = mutableListOf()
}
```

```kotlin
@Converter  
class ReferenceEmployeeAssociationConverter: AttributeConverter<Association<ReferenceEmployee, EmployeeId>, String> {  
    override fun convertToDatabaseColumn(association: Association<ReferenceEmployee, EmployeeId>): String {  
        return association.id.id  
    }  
  
    override fun convertToEntityAttribute(id: String): Association<ReferenceEmployee, EmployeeId> {  
        return Association.forId(EmployeeId(id))  
    }  

}
```