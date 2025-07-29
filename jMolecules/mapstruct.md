# MapStruct Mapping Problem - Zugriff auf jMolecules Identifier nicht möglich

Zu Beginn der Architektur-Migration war vorgesehen, MapStruct für das automatische Mapping von Domain-Objekten auf DTOs zu verwenden. 
Jedoch kam es zu Problemen beim Zugriff auf Identifikatoren und verschachtelte Aggregate Roots. Grund dafür war die Verwendung 
der Identifier von jMolecules, die Typen wie `ReferenceId`, `ChallengeId` oder `ImplementationId` kapseln. 
MapStruct konnte diese IDs nicht automatisch extrahieren, wodurch die generierten DTOs keine IDs oder nur fehlerhafte Strukturen enthielten.

## Code
```kotlin
data class ReferenceId(val id: String = UUID.randomUUID().toString()) : Identifier

//Mapper Ausschnitt
@Mapper(componentModel = "spring")
interface ReferenceMapper {
    @Mapping(target = "id", source = "reference.id.id")
    @Mapping(target = "state", source = "reference.state")
    @Mapping(target = "companyId", source = "reference.administrativeData.company.id")
    fun referenceToFullRefDto(reference: Reference): FullRefDto
}
```

### Stack Trace
```javastacktrace
// MapStruct-Fehlermeldungen
// Fehler 1: Kann Association<ReferenceEmployee,ReferenceEmployeeId> nicht zu String mappen
Can't map property "Association<ReferenceEmployee,ReferenceEmployeeId> notes[].createdBy" to "String notes[].createdBy".
Consider to declare/implement a mapping method: "String map(Association<ReferenceEmployee,ReferenceEmployeeId> value)".

// Fehler 2: Kann CompanyContactId nicht zu String mappen
Can't map property "CompanyContactId companyContact.id" to "String companyContact.id".
Consider to declare/implement a mapping method: "String map(CompanyContactId value)".

// Fehler 3: Kann CompanyDeputyContactId nicht zu String mappen
Can't map property "CompanyDeputyContactId companyDeputyContact.id" to "String companyDeputyContact.id".
Consider to declare/implement a mapping method: "String map(CompanyDeputyContactId value)".

etc. 
```

## Ursache
jMolecules verwendet sogenannte Identifier-Typen anstelle einfacher `UUID` oder `String`-Felder, z. B.:

Diese werden von MapStruct nicht automatisch aufgelöst, da MapStruct erwartet, dass die Felder direkt zugänglich sind.
Trotz der implementierung von mapping Methoden in MapStruct, die auf die `.id`-Eigenschaft zugreifen,
konnte MapStruct nicht korrekt auf die Identifier zugreifen.

## Lösung

Statt MapStruct wurde auf manuelles Mapping umgestellt, bei dem gezielt auf .id oder andere Properties zugegriffen wird,
um DTOs zu erzeugen. Durch das manuelle Mapping wurde sichergestellt, dass auch jMolecules-konforme 
Domain-Objekte zuverlässig in DTOs übersetzt werden.

### Code
```kotlin
// Manuelles Mapping
fun toDto(reference: Reference): ReferenceDto {
    return ReferenceDto(
        id = reference.id.id, // manuelle Auflösung von ReferenceId
    )
}
```