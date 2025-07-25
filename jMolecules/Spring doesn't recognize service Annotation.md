# Spring doesn't recognize service Annotation

Spring erkennt die @Service Annotationen von Jmolecules nicht und zeigt nur lokal einen Fehler an. Die Anwendung und Tests starten 
und laufen weiterhin. 

# Lösung

Eine manuelle Konfiguration für die Services löst das Problem.

```kotlin
@ComponentScan(
    basePackages = ["de.adesso.referencer"],
    // pick up jmolecules @Service as beans
    includeFilters = [
        ComponentScan.Filter(
            type  = FilterType.ANNOTATION,
            classes = [ Service::class ]
        )
    ]
)
```