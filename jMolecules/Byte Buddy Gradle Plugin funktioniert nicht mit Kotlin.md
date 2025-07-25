# ByteBuddy Gradle Plugin funktioniert nicht mit Kotlin
Entities und Repositories, welche die jMolecules Typen nutzen, werden nicht zu validen Spring Stereotypen umgewandelt. Deswegen lädt der Spring Context nicht, da eine `java.lang.IllegalStateException` geworfen.

## Code
```kotlin
buildscript {  
    repositories {  
       mavenCentral()  
    }  
    dependencies {  
       classpath(platform("org.jmolecules:jmolecules-bom:2023.2.1"))  
       classpath("org.jmolecules.integrations:jmolecules-bytebuddy")  
    }  
}  
  
plugins {  
    kotlin("jvm") version "1.9.25"  
    kotlin("plugin.spring") version "1.9.25"  
    id("org.springframework.boot") version "3.4.3"  
    id("io.spring.dependency-management") version "1.1.7"  
    kotlin("plugin.jpa") version "1.9.25"  
    id("net.bytebuddy.byte-buddy-gradle-plugin") version "1.17.2"  
    idea  
    jacoco  
}  
  
group = "de.adesso"  
version = "0.0.1-SNAPSHOT"  
  
java {  
    toolchain {  
       languageVersion = JavaLanguageVersion.of(21)  
    }  
}  
  
repositories {  
    mavenCentral()  
}  
  
extra["springCloudVersion"] = "2024.0.0"  
extra["springModulithVersion"] = "1.3.3"  
extra["jMoleculesVersion"] = "2023.2.1"  
extra["jMoleculesIntegrationVersion"] = "0.24.1"  
extra["mockkVersion"] = "1.13.17"  
extra["springMockkVersion"] = "4.0.2"  
  
dependencies {  
    //spring  
    implementation("org.springframework.boot:spring-boot-starter-actuator")  
    implementation("org.springframework.boot:spring-boot-starter-data-jpa")  
    implementation("org.springframework.boot:spring-boot-starter-freemarker")  
    implementation("org.springframework.boot:spring-boot-starter-oauth2-client")  
    implementation("org.springframework.boot:spring-boot-starter-security")  
    implementation("org.springframework.boot:spring-boot-starter-web")  
    implementation("org.springframework.boot:spring-boot-starter-webflux")  
    implementation("org.springframework.cloud:spring-cloud-starter-netflix-eureka-client")  
    implementation("org.springframework.cloud:spring-cloud-starter-openfeign")  
    implementation("org.springframework.kafka:spring-kafka")  
    implementation("org.springframework.modulith:spring-modulith-events-api")  
    implementation("org.springframework.modulith:spring-modulith-starter-core")  
    implementation("org.springframework.modulith:spring-modulith-starter-jpa")  
  
    //jmolecules  
    implementation("org.jmolecules:kmolecules-ddd")  
    implementation("org.jmolecules.integrations:jmolecules-starter-ddd")  
    implementation("org.jmolecules.integrations:jmolecules-bytebuddy")  
  
    //kotlin  
    implementation("io.projectreactor.kotlin:reactor-kotlin-extensions")  
    implementation("org.jetbrains.kotlin:kotlin-reflect")  
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-reactor")  
  
    //other  
    implementation("org.liquibase:liquibase-core")  
    implementation("com.fasterxml.jackson.module:jackson-module-kotlin")  
  
    //spring  
    runtimeOnly("org.springframework.modulith:spring-modulith-actuator")  
    runtimeOnly("org.springframework.modulith:spring-modulith-events-kafka")  
    runtimeOnly("org.springframework.modulith:spring-modulith-observability")  
  
    //jmolecules  
    runtimeOnly("org.jmolecules.integrations:jmolecules-jpa")  
    runtimeOnly("org.jmolecules.integrations:jmolecules-spring")  
  
    //other  
    runtimeOnly("org.mariadb.jdbc:mariadb-java-client")  
  
    //spring  
    testImplementation("org.springframework.boot:spring-boot-starter-test") {  
       exclude(module = "mockito-core")  
    }  
    testImplementation("org.springframework.boot:spring-boot-testcontainers")  
    testImplementation("org.springframework.kafka:spring-kafka-test")  
    testImplementation("org.springframework.modulith:spring-modulith-starter-test")  
    testImplementation("org.springframework.security:spring-security-test")  
  
    //kotlin  
    testImplementation("org.jetbrains.kotlin:kotlin-test-junit5")  
    testImplementation("org.jetbrains.kotlinx:kotlinx-coroutines-test")  
  
    //testcontainers  
    testImplementation("org.testcontainers:junit-jupiter")  
    testImplementation("org.testcontainers:redpanda")  
    testImplementation("org.testcontainers:mariadb")  
  
    //jmolecules  
    testImplementation("org.jmolecules.integrations:jmolecules-starter-test")  
    testImplementation("org.jmolecules.integrations:jmolecules-archunit")  
  
    //mockk  
    testImplementation("io.mockk:mockk:${property("mockkVersion")}")  
    testImplementation("com.ninja-squad:springmockk:${property("springMockkVersion")}")  
  
    //other  
    testImplementation("io.projectreactor:reactor-test")  
  
    testRuntimeOnly("org.junit.platform:junit-platform-launcher")  
}  
  
dependencyManagement {  
    imports {  
       mavenBom("org.springframework.modulith:spring-modulith-bom:${property("springModulithVersion")}")  
       mavenBom("org.springframework.cloud:spring-cloud-dependencies:${property("springCloudVersion")}")  
       mavenBom("org.jmolecules:jmolecules-bom:${property("jMoleculesVersion")}")  
    }  
}  
  
kotlin {  
    compilerOptions {  
       freeCompilerArgs.addAll("-Xjsr305=strict")  
    }  
}  
  
allOpen {  
    annotation("jakarta.persistence.Entity")  
    annotation("jakarta.persistence.MappedSuperclass")  
    annotation("jakarta.persistence.Embeddable")  
}  
  
jacoco{  
    toolVersion = "0.8.12"  
}  
  
tasks.withType<Test> {  
    useJUnitPlatform()  
    finalizedBy(tasks.jacocoTestReport)  
}  
  
tasks.jacocoTestReport {  
    dependsOn(tasks.test)  
    reports {  
       xml.required = true  
       html.required = true  
    }  
}  
  
byteBuddy {  
    transformation{  
       plugin = org.jmolecules.bytebuddy.JMoleculesPlugin::class.java  
    }  
}
```

### Stack Trace
```javastacktrace
java.lang.IllegalStateException: Failed to load ApplicationContext for [WebMergedContextConfiguration@5e161b43 testClass = de.adesso.referencer.ReferencerApplicationTests, locations = [], classes = [de.adesso.referencer.ReferencerApplication], contextInitializerClasses = [], activeProfiles = [], propertySourceDescriptors = [], propertySourceProperties = ["org.springframework.boot.test.context.SpringBootTestContextBootstrapper=true"], contextCustomizers = [[ImportsContextCustomizer@2860fccf key = [de.adesso.referencer.TestcontainersConfiguration]], org.springframework.boot.test.context.filter.ExcludeFilterContextCustomizer@24ac6fef, org.springframework.boot.test.json.DuplicateJsonObjectContextCustomizerFactory$DuplicateJsonObjectContextCustomizer@13f36d75, org.springframework.boot.test.mock.mockito.MockitoContextCustomizer@0, org.springframework.boot.test.web.client.TestRestTemplateContextCustomizer@cef885d, org.springframework.boot.test.web.reactor.netty.DisableReactorResourceFactoryGlobalResourcesContextCustomizerFactory$DisableReactorResourceFactoryGlobalResourcesContextCustomizerCustomizer@545f0b6, org.springframework.boot.test.autoconfigure.OnFailureConditionReportContextCustomizerFactory$OnFailureConditionReportContextCustomizer@3c8758d1, org.springframework.boot.test.autoconfigure.actuate.observability.ObservabilityContextCustomizerFactory$DisableObservabilityContextCustomizer@1f, org.springframework.boot.test.autoconfigure.properties.PropertyMappingContextCustomizer@0, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverContextCustomizer@5f82209e, org.springframework.test.context.support.DynamicPropertiesContextCustomizer@0, org.springframework.boot.testcontainers.service.connection.ServiceConnectionContextCustomizer@0, org.springframework.boot.test.context.SpringBootTestAnnotation@ec19e835], resourceBasePath = "src/main/webapp", contextLoader = org.springframework.boot.test.context.SpringBootContextLoader, parent = null]

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
Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'healthContributorRegistry' defined in class path resource [org/springframework/boot/actuate/autoconfigure/health/HealthEndpointConfiguration.class]: Unsatisfied dependency expressed through method 'healthContributorRegistry' parameter 2: Error creating bean with name 'dbHealthContributor' defined in class path resource [org/springframework/boot/actuate/autoconfigure/jdbc/DataSourceHealthContributorAutoConfiguration.class]: Failed to instantiate [org.springframework.boot.actuate.health.HealthContributor]: Factory method 'dbHealthContributor' threw exception with message: Error creating bean with name 'companyRepository' defined in de.adesso.referencer.companies.internal.CompanyRepository defined in @EnableJpaRepositories declared on JpaRepositoriesRegistrar.EnableJpaRepositoriesConfiguration: Not a managed type: class de.adesso.referencer.companies.internal.Company
	at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:804)
	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:546)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1361)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1191)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:563)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:523)
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:339)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:347)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:337)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:202)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.instantiateSingleton(DefaultListableBeanFactory.java:1155)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingleton(DefaultListableBeanFactory.java:1121)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:1056)
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:987)
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
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'dbHealthContributor' defined in class path resource [org/springframework/boot/actuate/autoconfigure/jdbc/DataSourceHealthContributorAutoConfiguration.class]: Failed to instantiate [org.springframework.boot.actuate.health.HealthContributor]: Factory method 'dbHealthContributor' threw exception with message: Error creating bean with name 'companyRepository' defined in de.adesso.referencer.companies.internal.CompanyRepository defined in @EnableJpaRepositories declared on JpaRepositoriesRegistrar.EnableJpaRepositoriesConfiguration: Not a managed type: class de.adesso.referencer.companies.internal.Company
	at org.springframework.beans.factory.support.ConstructorResolver.instantiate(ConstructorResolver.java:657)
	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:645)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1361)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1191)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:563)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:523)
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:339)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:347)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:337)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:202)
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:254)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.addCandidateEntry(DefaultListableBeanFactory.java:1919)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.findAutowireCandidates(DefaultListableBeanFactory.java:1883)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveMultipleBeanMap(DefaultListableBeanFactory.java:1805)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveMultipleBeans(DefaultListableBeanFactory.java:1744)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1616)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1555)
	at org.springframework.beans.factory.support.ConstructorResolver.resolveAutowiredArgument(ConstructorResolver.java:913)
	at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:791)
	... 45 more
Caused by: org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.springframework.boot.actuate.health.HealthContributor]: Factory method 'dbHealthContributor' threw exception with message: Error creating bean with name 'companyRepository' defined in de.adesso.referencer.companies.internal.CompanyRepository defined in @EnableJpaRepositories declared on JpaRepositoriesRegistrar.EnableJpaRepositoriesConfiguration: Not a managed type: class de.adesso.referencer.companies.internal.Company
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.lambda$instantiate$0(SimpleInstantiationStrategy.java:199)
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiateWithFactoryMethod(SimpleInstantiationStrategy.java:88)
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:168)
	at org.springframework.beans.factory.support.ConstructorResolver.instantiate(ConstructorResolver.java:653)
	... 63 more
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'companyRepository' defined in de.adesso.referencer.companies.internal.CompanyRepository defined in @EnableJpaRepositories declared on JpaRepositoriesRegistrar.EnableJpaRepositoriesConfiguration: Not a managed type: class de.adesso.referencer.companies.internal.Company
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1812)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:601)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:523)
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:339)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:347)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:337)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:202)
	at org.springframework.beans.factory.support.AbstractBeanFactory.isSingleton(AbstractBeanFactory.java:470)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.isSingleton(DefaultListableBeanFactory.java:693)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doGetBeanNamesForType(DefaultListableBeanFactory.java:630)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.getBeanNamesForType(DefaultListableBeanFactory.java:598)
	at org.springframework.beans.factory.BeanFactoryUtils.beanNamesForTypeIncludingAncestors(BeanFactoryUtils.java:264)
	at org.springframework.beans.factory.support.SimpleAutowireCandidateResolver.resolveAutowireCandidates(SimpleAutowireCandidateResolver.java:101)
	at org.springframework.boot.actuate.autoconfigure.jdbc.DataSourceHealthContributorAutoConfiguration.dbHealthContributor(DataSourceHealthContributorAutoConfiguration.java:91)
	at java.base/java.lang.reflect.Method.invoke(Method.java:580)
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.lambda$instantiate$0(SimpleInstantiationStrategy.java:171)
	... 66 more
Caused by: java.lang.IllegalArgumentException: Not a managed type: class de.adesso.referencer.companies.internal.Company
	at org.hibernate.metamodel.model.domain.internal.JpaMetamodelImpl.managedType(JpaMetamodelImpl.java:223)
	at org.hibernate.metamodel.model.domain.internal.MappingMetamodelImpl.managedType(MappingMetamodelImpl.java:470)
	at org.hibernate.metamodel.model.domain.internal.MappingMetamodelImpl.managedType(MappingMetamodelImpl.java:100)
	at org.springframework.data.jpa.repository.support.JpaMetamodelEntityInformation.<init>(JpaMetamodelEntityInformation.java:82)
	at org.springframework.data.jpa.repository.support.JpaEntityInformationSupport.getEntityInformation(JpaEntityInformationSupport.java:69)
	at org.springframework.data.jpa.repository.support.JpaRepositoryFactory.getEntityInformation(JpaRepositoryFactory.java:251)
	at org.springframework.data.jpa.repository.support.JpaRepositoryFactory.getTargetRepository(JpaRepositoryFactory.java:215)
	at org.springframework.data.jpa.repository.support.JpaRepositoryFactory.getTargetRepository(JpaRepositoryFactory.java:198)
	at org.springframework.data.jpa.repository.support.JpaRepositoryFactory.getTargetRepository(JpaRepositoryFactory.java:1)
	at org.springframework.data.repository.core.support.RepositoryFactorySupport.getRepository(RepositoryFactorySupport.java:377)
	at org.springframework.data.repository.core.support.RepositoryFactoryBeanSupport.lambda$afterPropertiesSet$4(RepositoryFactoryBeanSupport.java:350)
	at org.springframework.data.util.Lazy.getNullable(Lazy.java:135)
	at org.springframework.data.util.Lazy.get(Lazy.java:113)
	at org.springframework.data.repository.core.support.RepositoryFactoryBeanSupport.afterPropertiesSet(RepositoryFactoryBeanSupport.java:356)
	at org.springframework.data.jpa.repository.support.JpaRepositoryFactoryBean.afterPropertiesSet(JpaRepositoryFactoryBean.java:132)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.invokeInitMethods(AbstractAutowireCapableBeanFactory.java:1859)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1808)
	... 81 more
```

## Ursache
Die ByteBuddy Plugin für Gradle unterstüzt [unterstüzt Kotlin nicht](https://github.com/raphw/byte-buddy/issues/1284#issuecomment-1191991216) . Da es ein ByteBuddy Problem ist, wird das Problem auch [nicht direkt in jMolecules gefixt](https://github.com/xmolecules/jmolecules-integrations/issues/199).

## Lösung
### Mögliche Lösungen
- ByteBuddy Task modifizieren
- Eigene ByteBuddy Plugin schreiben
- ByteBuddy um Kotlin support erweitern
- Maven nutzen
- Java nutzen

### Gewählte Lösung
Maven nutzen.
