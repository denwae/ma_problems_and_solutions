# jMolecules `@Service` Annotation - Die Anwendung startet nicht

## Meta
Status: akzeptiert, dokumentiert – siehe https://github.com/xmolecules/jmolecules-integrations/tree/main/jmolecules-bytebuddy#kotlin-specialties

## Zusammenfassung
Wenn die JMolecules `@Service` Annotation genutzt wird, dann kann die Anwendung nicht starten. Dabei wird eine `org.springframework.beans.factory.BeanCreationException` geworfen.

## Code
```kotlin
@Service  
class EmployeeLegacyListener(  
    val repository: EmployeeRepository  
) {}
```

### Stack Trace
```javastacktrace
Error starting ApplicationContext. To display the condition evaluation report re-run your application with 'debug' enabled.
2025-04-07T12:24:27.283+02:00 ERROR 2172 --- [referencer] [  restartedMain] [                                                 ] o.s.boot.SpringApplication               : Application run failed

org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'employeeLegacyListener' defined in file [C:\Users\waeckerle\IdeaProjects\referencer\modulith\target\classes\de\adesso\referencer\employees\internal\EmployeeLegacyListener.class]: Could not generate CGLIB subclass of class de.adesso.referencer.employees.internal.EmployeeLegacyListener: Common causes of this problem include using a final class or a non-visible class
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:608) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:523) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:339) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:347) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:337) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:202) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.instantiateSingleton(DefaultListableBeanFactory.java:1155) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingleton(DefaultListableBeanFactory.java:1121) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:1056) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:987) ~[spring-context-6.2.5.jar:6.2.5]
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
Caused by: org.springframework.aop.framework.AopConfigException: Could not generate CGLIB subclass of class de.adesso.referencer.employees.internal.EmployeeLegacyListener: Common causes of this problem include using a final class or a non-visible class
	at org.springframework.aop.framework.CglibAopProxy.buildProxy(CglibAopProxy.java:237) ~[spring-aop-6.2.5.jar:6.2.5]
	at org.springframework.aop.framework.CglibAopProxy.getProxy(CglibAopProxy.java:167) ~[spring-aop-6.2.5.jar:6.2.5]
	at org.springframework.aop.framework.ProxyFactory.getProxy(ProxyFactory.java:110) ~[spring-aop-6.2.5.jar:6.2.5]
	at org.springframework.modulith.observability.ModuleTracingSupport.addAdvisor(ModuleTracingSupport.java:48) ~[spring-modulith-observability-1.3.3.jar:1.3.3]
	at org.springframework.modulith.observability.ModuleTracingSupport.addAdvisor(ModuleTracingSupport.java:31) ~[spring-modulith-observability-1.3.3.jar:1.3.3]
	at org.springframework.modulith.observability.ModuleTracingBeanPostProcessor.lambda$postProcessAfterInitialization$0(ModuleTracingBeanPostProcessor.java:107) ~[spring-modulith-observability-1.3.3.jar:1.3.3]
	at java.base/java.util.Optional.map(Optional.java:260) ~[na:na]
	at org.springframework.modulith.observability.ModuleTracingBeanPostProcessor.postProcessAfterInitialization(ModuleTracingBeanPostProcessor.java:102) ~[spring-modulith-observability-1.3.3.jar:1.3.3]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.applyBeanPostProcessorsAfterInitialization(AbstractAutowireCapableBeanFactory.java:439) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1815) ~[spring-beans-6.2.5.jar:6.2.5]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:601) ~[spring-beans-6.2.5.jar:6.2.5]
	... 20 common frames omitted
Caused by: java.lang.IllegalArgumentException: Cannot subclass final class de.adesso.referencer.employees.internal.EmployeeLegacyListener
	at org.springframework.cglib.proxy.Enhancer.generateClass(Enhancer.java:653) ~[spring-core-6.2.5.jar:6.2.5]
	at org.springframework.cglib.core.DefaultGeneratorStrategy.generate(DefaultGeneratorStrategy.java:26) ~[spring-core-6.2.5.jar:6.2.5]
	at org.springframework.cglib.core.ClassLoaderAwareGeneratorStrategy.lambda$new$0(ClassLoaderAwareGeneratorStrategy.java:41) ~[spring-core-6.2.5.jar:6.2.5]
	at org.springframework.cglib.core.ClassLoaderAwareGeneratorStrategy.generate(ClassLoaderAwareGeneratorStrategy.java:76) ~[spring-core-6.2.5.jar:6.2.5]
	at org.springframework.cglib.core.AbstractClassGenerator.generate(AbstractClassGenerator.java:370) ~[spring-core-6.2.5.jar:6.2.5]
	at org.springframework.cglib.proxy.Enhancer.generate(Enhancer.java:575) ~[spring-core-6.2.5.jar:6.2.5]
	at org.springframework.cglib.core.AbstractClassGenerator$ClassLoaderData.get(AbstractClassGenerator.java:133) ~[spring-core-6.2.5.jar:6.2.5]
	at org.springframework.cglib.core.AbstractClassGenerator.create(AbstractClassGenerator.java:321) ~[spring-core-6.2.5.jar:6.2.5]
	at org.springframework.cglib.proxy.Enhancer.createHelper(Enhancer.java:562) ~[spring-core-6.2.5.jar:6.2.5]
	at org.springframework.cglib.proxy.Enhancer.createClass(Enhancer.java:407) ~[spring-core-6.2.5.jar:6.2.5]
	at org.springframework.aop.framework.ObjenesisCglibAopProxy.createProxyClassAndInstance(ObjenesisCglibAopProxy.java:62) ~[spring-aop-6.2.5.jar:6.2.5]
	at org.springframework.aop.framework.CglibAopProxy.buildProxy(CglibAopProxy.java:228) ~[spring-aop-6.2.5.jar:6.2.5]
	... 30 common frames omitted
```

## Ursache
Alle Kotlin Klassen sind standardmäßig `final` und können nicht von Spring erweitert werden.

## Lösung
Die Klasse `open` machen oder die Kotlin All Open Plugin für JMolecules Services einstellen.

### Code
```kotlin
@Service  
open class EmployeeLegacyListener(  
    val repository: EmployeeRepository  
) {}
```

```xml
<pluginOptions>
	<!-- skipped for brevity -->
	<option>all-open:annotation=org.jmolecules.ddd.annotation.Service</option>
</pluginOptions>
```
