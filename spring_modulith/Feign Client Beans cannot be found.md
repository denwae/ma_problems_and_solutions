# Feign Client Beans können nicht gefunden werden

Wenn Feign Clients nicht im Shared Module definiert sind, werden diese bei einem ApplicationModuleTest nicht gefunden und Spring wirft einen Fehler.

## Code
### Stack Trace

```
Description:
Method feignClientBeanFactoryInitializationCodeGenerator in org.springframework.cloud.openfeign.FeignAutoConfiguration required a bean named 'de.adesso.referencer.companies.internal.infrastructure.feign.ConvertClient' that could not be found.

Action:
Consider defining a bean named 'de.adesso.referencer.companies.internal.infrastructure.feign.ConvertClient' in your configuration.
```

## Ursache

Da `@EnableFeignClients` innerhalb der ReferencerApplication definiert wurde, versucht Spring jedes Mal alle FeignClients zu laden. Wenn nur einzelne Module gebootstrapped werden, findet Spring die Bean nicht und wirft einen Fehler.

# Lösung

Eine Configuration innerhalb jedes Modules erstellen, in dem die `@FeignClientsEnabled` annotiert ist, um nur die FeignClients innerhalb des Moduls zu laden und die globale Configuration entfernen.

```kotlin
//Global:
package de.adesso.referencer
import org.springframework.boot.context.properties.ConfigurationPropertiesScan
import org.springframework.boot.runApplication
import org.springframework.modulith.Modulith

@Modulith(sharedModules = ["core"])
@ConfigurationPropertiesScan
class ReferencerApplication {

	fun main(args: Array<String>) {
		runApplication<ReferencerApplication>(*args)
	}

}
----------------------------------------------------------------------------------------------------

// Modul:

package de.adesso.referencer.companies
import org.springframework.cloud.openfeign.EnableFeignClients
import org.springframework.context.annotation.Configuration
import org.springframework.modulith.ApplicationModule

@ApplicationModule(
	id = "companies",
	allowedDependencies = []
)
@Configuration
@EnableFeignClients
class CompaniesModule {

}

```