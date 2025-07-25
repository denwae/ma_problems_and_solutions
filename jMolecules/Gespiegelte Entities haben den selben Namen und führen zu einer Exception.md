# Gespiegelte Entities - `BeanCreationException`
Wenn Entities in einem anderen Package gespiegelt werden, wird eine `org.springframework.beans.factory.BeanCreationException` geworfen.

## Stack Trace
```javastacktrace
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: Entity classes [de.adesso.referencer.dataCreation.internal.foreign.Employee] and [de.adesso.referencer.employees.internal.Employee] share the entity name 'Employee' (entity names must be distinct)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1812) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:601) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:523) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:339) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:347) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:337) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:207) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:970) ~[spring-context-6.2.5.jar:6.2.5]
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:627) ~[spring-context-6.2.5.jar:6.2.5]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:146) ~[spring-boot-3.4.4.jar:3.4.4]
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:752) ~[spring-boot-3.4.4.jar:3.4.4]
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:439) ~[spring-boot-3.4.4.jar:3.4.4]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:318) ~[spring-boot-3.4.4.jar:3.4.4]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1361) ~[spring-boot-3.4.4.jar:3.4.4]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1350) ~[spring-boot-3.4.4.jar:3.4.4]
	at de.adesso.referencer.ReferencerApplicationKt.main(ReferencerApplication.kt:13) ~[classes/:na]
	at java.base/jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:103) ~[na:na]
	at java.base/java.lang.reflect.Method.invoke(Method.java:580) ~[na:na]
	at org.springframework.boot.devtools.restart.RestartLauncher.run(RestartLauncher.java:50) ~[spring-boot-devtools-3.4.4.jar:3.4.4]
Caused by: org.hibernate.DuplicateMappingException: Entity classes [de.adesso.referencer.dataCreation.internal.foreign.Employee] and [de.adesso.referencer.employees.internal.Employee] share the entity name 'Employee' (entity names must be distinct)
	at org.hibernate.boot.internal.InFlightMetadataCollectorImpl.addEntityBinding(InFlightMetadataCollectorImpl.java:360) ~[hibernate-core-6.6.11.Final.jar:6.6.11.Final]
	at org.hibernate.boot.model.internal.EntityBinder.bindEntityClass(EntityBinder.java:262) ~[hibernate-core-6.6.11.Final.jar:6.6.11.Final]
	at org.hibernate.boot.model.internal.AnnotationBinder.bindClass(AnnotationBinder.java:401) ~[hibernate-core-6.6.11.Final.jar:6.6.11.Final]
	at org.hibernate.boot.model.source.internal.annotations.AnnotationMetadataSourceProcessorImpl.processEntityHierarchies(AnnotationMetadataSourceProcessorImpl.java:257) ~[hibernate-core-6.6.11.Final.jar:6.6.11.Final]
	at org.hibernate.boot.model.process.spi.MetadataBuildingProcess$1.processEntityHierarchies(MetadataBuildingProcess.java:281) ~[hibernate-core-6.6.11.Final.jar:6.6.11.Final]
	at org.hibernate.boot.model.process.spi.MetadataBuildingProcess.complete(MetadataBuildingProcess.java:324) ~[hibernate-core-6.6.11.Final.jar:6.6.11.Final]
	at org.hibernate.jpa.boot.internal.EntityManagerFactoryBuilderImpl.metadata(EntityManagerFactoryBuilderImpl.java:1442) ~[hibernate-core-6.6.11.Final.jar:6.6.11.Final]
	at org.hibernate.jpa.boot.internal.EntityManagerFactoryBuilderImpl.build(EntityManagerFactoryBuilderImpl.java:1513) ~[hibernate-core-6.6.11.Final.jar:6.6.11.Final]
	at org.springframework.orm.jpa.vendor.SpringHibernateJpaPersistenceProvider.createContainerEntityManagerFactory(SpringHibernateJpaPersistenceProvider.java:66) ~[spring-orm-6.2.5.jar:6.2.5]
	at org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean.createNativeEntityManagerFactory(LocalContainerEntityManagerFactoryBean.java:390) ~[spring-orm-6.2.5.jar:6.2.5]
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.buildNativeEntityManagerFactory(AbstractEntityManagerFactoryBean.java:419) ~[spring-orm-6.2.5.jar:6.2.5]
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.afterPropertiesSet(AbstractEntityManagerFactoryBean.java:400) ~[spring-orm-6.2.5.jar:6.2.5]
	at org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean.afterPropertiesSet(LocalContainerEntityManagerFactoryBean.java:366) ~[spring-orm-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.invokeInitMethods(AbstractAutowireCapableBeanFactory.java:1859) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1808) ~[spring-beans-6.2.5.jar:6.2.5]
```

## Ursache
Wenn die jMolecules ByteBuddy Plugin die Entities umwandelt werden, diese mit `@Entitiy` annotiert. Diese Entities haben denselben Namen, was von 

# Lösung
Entities umbenennen.
