# Zu viele Application Module Tests - Timeouts beim starten von Testcontainer

Beim durchführen von Tests schlagen ab einem gewissen Punkt Tests fehl, weil die nötigen Test Container, wegen eines Timeout, nicht mehr gestartet werden können. 

## Code
```kotlin
@TestConfiguration(proxyBeanMethods = false)  
class TestcontainersConfiguration {  
  
    @Bean  
    @ServiceConnection    fun redpandaContainer(): RedpandaContainer {  
       return RedpandaContainer("docker.redpanda.com/redpandadata/redpanda:v23.1.7")  
    }  
  
    @Bean  
    @ServiceConnection    fun mariaDbContainer(): MariaDBContainer<*> {  
       return MariaDBContainer(DockerImageName  
          .parse("artifactory.adesso-group.com/public-docker/mariadb:10.11.11")  
          .asCompatibleSubstituteFor("mariadb")  
       ).apply {  
             withExposedPorts(3306)  
             withDatabaseName("referencer")  
             withPassword("root")  
             withCommand("mysqld", "--max-allowed-packet=10M")  
          }  
    }  
  
}
```

### Stack Trace
```javastacktrace
025-04-09T19:46:08.467+02:00 ERROR 36324 --- [referencer] [           main] [                                                 ] t.d.r.com/redpandadata/redpanda:v24.3.9  : Could not start container

java.lang.IllegalStateException: Wait strategy failed. Container exited with code 139
	at org.testcontainers.containers.GenericContainer.tryStart(GenericContainer.java:525) ~[testcontainers-1.20.6.jar:na]
	at org.testcontainers.containers.GenericContainer.lambda$doStart$0(GenericContainer.java:346) ~[testcontainers-1.20.6.jar:na]
	at org.rnorth.ducttape.unreliables.Unreliables.retryUntilSuccess(Unreliables.java:81) ~[duct-tape-1.0.8.jar:na]
	at org.testcontainers.containers.GenericContainer.doStart(GenericContainer.java:336) ~[testcontainers-1.20.6.jar:na]
	at org.testcontainers.containers.GenericContainer.start(GenericContainer.java:322) ~[testcontainers-1.20.6.jar:na]
	at org.springframework.boot.testcontainers.lifecycle.TestcontainersStartup.start(TestcontainersStartup.java:108) ~[spring-boot-testcontainers-3.4.4.jar:3.4.4]
	at java.base/java.lang.Iterable.forEach(Iterable.java:75) ~[na:na]
	at org.springframework.boot.testcontainers.lifecycle.TestcontainersStartup$1.start(TestcontainersStartup.java:49) ~[spring-boot-testcontainers-3.4.4.jar:3.4.4]
	at org.springframework.boot.testcontainers.lifecycle.TestcontainersLifecycleBeanPostProcessor.start(TestcontainersLifecycleBeanPostProcessor.java:135) ~[spring-boot-testcontainers-3.4.4.jar:3.4.4]
	at org.springframework.boot.testcontainers.lifecycle.TestcontainersLifecycleBeanPostProcessor.initializeStartables(TestcontainersLifecycleBeanPostProcessor.java:123) ~[spring-boot-testcontainers-3.4.4.jar:3.4.4]
	at org.springframework.boot.testcontainers.lifecycle.TestcontainersLifecycleBeanPostProcessor.postProcessAfterInitialization(TestcontainersLifecycleBeanPostProcessor.java:96) ~[spring-boot-testcontainers-3.4.4.jar:3.4.4]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.applyBeanPostProcessorsAfterInitialization(AbstractAutowireCapableBeanFactory.java:439) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1815) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:601) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:523) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:339) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:347) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:337) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:202) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:254) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1667) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1555) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.ConstructorResolver.resolveAutowiredArgument(ConstructorResolver.java:913) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:791) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:546) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1361) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1191) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:563) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:523) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:339) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:347) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:337) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:207) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.test.context.support.DynamicPropertyRegistrarBeanInitializer.initialize(DynamicPropertyRegistrarBeanInitializer.java:78) ~[spring-test-6.2.5.jar:6.2.5]
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:963) ~[spring-context-6.2.5.jar:6.2.5]
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:627) ~[spring-context-6.2.5.jar:6.2.5]
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:752) ~[spring-boot-3.4.4.jar:3.4.4]
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:439) ~[spring-boot-3.4.4.jar:3.4.4]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:318) ~[spring-boot-3.4.4.jar:3.4.4]
	at org.springframework.boot.test.context.SpringBootContextLoader.lambda$loadContext$3(SpringBootContextLoader.java:144) ~[spring-boot-test-3.4.4.jar:3.4.4]
	at org.springframework.util.function.ThrowingSupplier.get(ThrowingSupplier.java:58) ~[spring-core-6.2.5.jar:6.2.5]
	at org.springframework.util.function.ThrowingSupplier.get(ThrowingSupplier.java:46) ~[spring-core-6.2.5.jar:6.2.5]
	at org.springframework.boot.SpringApplication.withHook(SpringApplication.java:1461) ~[spring-boot-3.4.4.jar:3.4.4]
	at org.springframework.boot.test.context.SpringBootContextLoader$ContextLoaderHook.run(SpringBootContextLoader.java:563) ~[spring-boot-test-3.4.4.jar:3.4.4]
	at org.springframework.boot.test.context.SpringBootContextLoader.loadContext(SpringBootContextLoader.java:144) ~[spring-boot-test-3.4.4.jar:3.4.4]
	at org.springframework.boot.test.context.SpringBootContextLoader.loadContext(SpringBootContextLoader.java:110) ~[spring-boot-test-3.4.4.jar:3.4.4]
	at org.springframework.test.context.cache.DefaultCacheAwareContextLoaderDelegate.loadContextInternal(DefaultCacheAwareContextLoaderDelegate.java:225) ~[spring-test-6.2.5.jar:6.2.5]
	at org.springframework.test.context.cache.DefaultCacheAwareContextLoaderDelegate.loadContext(DefaultCacheAwareContextLoaderDelegate.java:152) ~[spring-test-6.2.5.jar:6.2.5]
	at org.springframework.test.context.support.DefaultTestContext.getApplicationContext(DefaultTestContext.java:130) ~[spring-test-6.2.5.jar:6.2.5]
	at org.springframework.test.context.junit.jupiter.SpringExtension.getApplicationContext(SpringExtension.java:352) ~[spring-test-6.2.5.jar:6.2.5]
	at org.springframework.test.context.junit.jupiter.SpringExtension.resolveParameter(SpringExtension.java:338) ~[spring-test-6.2.5.jar:6.2.5]
	at org.junit.jupiter.engine.execution.ParameterResolutionUtils.resolveParameter(ParameterResolutionUtils.java:136) ~[junit-jupiter-engine-5.11.4.jar:5.11.4]
	at org.junit.jupiter.engine.execution.ParameterResolutionUtils.resolveParameters(ParameterResolutionUtils.java:103) ~[junit-jupiter-engine-5.11.4.jar:5.11.4]
	at org.junit.jupiter.engine.execution.InterceptingExecutableInvoker.invoke(InterceptingExecutableInvoker.java:59) ~[junit-jupiter-engine-5.11.4.jar:5.11.4]
	at org.junit.jupiter.engine.descriptor.ClassBasedTestDescriptor.invokeTestClassConstructor(ClassBasedTestDescriptor.java:364) ~[junit-jupiter-engine-5.11.4.jar:5.11.4]
	at org.junit.jupiter.engine.descriptor.ClassBasedTestDescriptor.instantiateTestClass(ClassBasedTestDescriptor.java:311) ~[junit-jupiter-engine-5.11.4.jar:5.11.4]
	at org.junit.jupiter.engine.descriptor.ClassTestDescriptor.instantiateTestClass(ClassTestDescriptor.java:79) ~[junit-jupiter-engine-5.11.4.jar:5.11.4]
	at org.junit.jupiter.engine.descriptor.ClassBasedTestDescriptor.instantiateAndPostProcessTestInstance(ClassBasedTestDescriptor.java:287) ~[junit-jupiter-engine-5.11.4.jar:5.11.4]
	at org.junit.jupiter.engine.descriptor.ClassBasedTestDescriptor.lambda$testInstancesProvider$5(ClassBasedTestDescriptor.java:279) ~[junit-jupiter-engine-5.11.4.jar:5.11.4]
	at java.base/java.util.Optional.orElseGet(Optional.java:364) ~[na:na]
	at org.junit.jupiter.engine.descriptor.ClassBasedTestDescriptor.lambda$testInstancesProvider$6(ClassBasedTestDescriptor.java:278) ~[junit-jupiter-engine-5.11.4.jar:5.11.4]
	at org.junit.jupiter.engine.execution.TestInstancesProvider.getTestInstances(TestInstancesProvider.java:31) ~[junit-jupiter-engine-5.11.4.jar:5.11.4]
	at org.junit.jupiter.engine.descriptor.ClassBasedTestDescriptor.lambda$before$3(ClassBasedTestDescriptor.java:204) ~[junit-jupiter-engine-5.11.4.jar:5.11.4]
	at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.jupiter.engine.descriptor.ClassBasedTestDescriptor.before(ClassBasedTestDescriptor.java:203) ~[junit-jupiter-engine-5.11.4.jar:5.11.4]
	at org.junit.jupiter.engine.descriptor.ClassBasedTestDescriptor.before(ClassBasedTestDescriptor.java:85) ~[junit-jupiter-engine-5.11.4.jar:5.11.4]
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$6(NodeTestTask.java:153) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$8(NodeTestTask.java:146) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.Node.around(Node.java:137) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$9(NodeTestTask.java:144) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.executeRecursively(NodeTestTask.java:143) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.execute(NodeTestTask.java:100) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at java.base/java.util.ArrayList.forEach(ArrayList.java:1596) ~[na:na]
	at org.junit.platform.engine.support.hierarchical.SameThreadHierarchicalTestExecutorService.invokeAll(SameThreadHierarchicalTestExecutorService.java:41) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$6(NodeTestTask.java:160) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$8(NodeTestTask.java:146) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.Node.around(Node.java:137) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$9(NodeTestTask.java:144) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.executeRecursively(NodeTestTask.java:143) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.execute(NodeTestTask.java:100) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.SameThreadHierarchicalTestExecutorService.submit(SameThreadHierarchicalTestExecutorService.java:35) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.HierarchicalTestExecutor.execute(HierarchicalTestExecutor.java:57) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.engine.support.hierarchical.HierarchicalTestEngine.execute(HierarchicalTestEngine.java:54) ~[junit-platform-engine-1.11.4.jar:1.11.4]
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:198) ~[junit-platform-launcher-1.11.4.jar:1.11.4]
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:169) ~[junit-platform-launcher-1.11.4.jar:1.11.4]
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:93) ~[junit-platform-launcher-1.11.4.jar:1.11.4]
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.lambda$execute$0(EngineExecutionOrchestrator.java:58) ~[junit-platform-launcher-1.11.4.jar:1.11.4]
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.withInterceptedStreams(EngineExecutionOrchestrator.java:141) ~[junit-platform-launcher-1.11.4.jar:1.11.4]
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:57) ~[junit-platform-launcher-1.11.4.jar:1.11.4]
	at org.junit.platform.launcher.core.DefaultLauncher.execute(DefaultLauncher.java:103) ~[junit-platform-launcher-1.11.4.jar:1.11.4]
	at org.junit.platform.launcher.core.DefaultLauncher.execute(DefaultLauncher.java:85) ~[junit-platform-launcher-1.11.4.jar:1.11.4]
	at org.junit.platform.launcher.core.DelegatingLauncher.execute(DelegatingLauncher.java:47) ~[junit-platform-launcher-1.11.4.jar:1.11.4]
	at org.junit.platform.launcher.core.SessionPerRequestLauncher.execute(SessionPerRequestLauncher.java:63) ~[junit-platform-launcher-1.11.4.jar:1.11.4]
	at com.intellij.junit5.JUnit5IdeaTestRunner.startRunnerWithArgs(JUnit5IdeaTestRunner.java:57) ~[junit5-rt.jar:na]
	at com.intellij.rt.junit.IdeaTestRunner$Repeater$1.execute(IdeaTestRunner.java:38) ~[junit-rt.jar:na]
	at com.intellij.rt.execution.junit.TestsRepeater.repeat(TestsRepeater.java:11) ~[idea_rt.jar:na]
	at com.intellij.rt.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:35) ~[junit-rt.jar:na]
	at com.intellij.rt.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:232) ~[junit-rt.jar:na]
	at com.intellij.rt.junit.JUnitStarter.main(JUnitStarter.java:55) ~[junit-rt.jar:na]
Caused by: org.testcontainers.containers.ContainerLaunchException: Timed out waiting for log output matching '.*Successfully started Redpanda!.*'
	at org.testcontainers.containers.wait.strategy.LogMessageWaitStrategy.waitUntilReady(LogMessageWaitStrategy.java:47) ~[testcontainers-1.20.6.jar:1.20.6]
	at org.testcontainers.containers.wait.strategy.AbstractWaitStrategy.waitUntilReady(AbstractWaitStrategy.java:52) ~[testcontainers-1.20.6.jar:1.20.6]
	at org.testcontainers.containers.GenericContainer.waitUntilContainerStarted(GenericContainer.java:909) ~[testcontainers-1.20.6.jar:na]
	at org.testcontainers.containers.GenericContainer.tryStart(GenericContainer.java:492) ~[testcontainers-1.20.6.jar:na]
	... 102 common frames omitted
```

## Ursache
Alle Testklassen, die mit `@ApplicationModuleTest` annotiert sind, scheinen einen neuen Spring Context zu erstellen. Wenn Testcontainers genutzt werden, wird für jeden Spring Context neue Container erstellt und nicht mehr aufgeräumt, bis alle Tests durchgelaufen sind

## Lösung

JUnit5 Lifecycle Methoden nutzen, um die Container nach den Tests zu beenden. Für weniger Codeduplizierung kann eine neue Klasse mit den passenden Methoden erstellt werden, von der die anderen Klassen erben.

### Code
```kotlin
@ActiveProfiles("test")  
@Import(TestcontainersConfiguration::class)  
abstract class AbstractIntegrationTest(  
    mariaDBContainer: MariaDBContainer<*>,  
    redpandaContainer: RedpandaContainer,  
) {  
  
    init {  
        companionMariaDBContainer = mariaDBContainer  
        companionRedpandaContainer = redpandaContainer  
    }  
  
    companion object {  
        lateinit var companionMariaDBContainer: MariaDBContainer<*>  
        lateinit var companionRedpandaContainer: RedpandaContainer  
  
        @JvmStatic  
        @AfterAll        
        fun shutdownContainers(): Unit {  
            companionMariaDBContainer.stop()  
            companionRedpandaContainer.stop()  
        }  
    }  
}
```

```kotlin
@ApplicationModuleTest(module = "dataCreation")  
@Transactional  
class CompanyRoleRepositoryTest(  
    val repository: CompanyRoleRepository,  
    mariaDBContainer: MariaDBContainer<*>,  
    redpandaContainer: RedpandaContainer  
): AbstractIntegrationTest(mariaDBContainer, redpandaContainer)
```
