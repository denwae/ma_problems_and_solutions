# jMolecules Associations können nicht richtig konvertiert werden
Beim Update von `ReferenceCompany` wird der `ReferenceEmployeeAssociationConverter` genutzt, anstelle vom definierten Converter, für die `requiredField`

## Code
```kotlin
class ReferenceCompany(  
    override val id: CompanyId,  
    var name: String,  
    var logo: ByteArray,  
    var url: String,  
    @Association
    @Convert(converter = RequiredFieldIdConverter::class)  
    var requiredField: MutableList<Association<ReferenceRequiredField, RequiredFieldId>>,  
    var wordTemplate: ByteArray?,  
    var powerPointTemplate: ByteArray?,  
): AggregateRoot<ReferenceCompany, CompanyId>
```

## Ursache
Hibernate scheint mit unterschiedlichen `AttributeConverter` des selben Typen mit unterschiedlichen Generics nicht zurecht zu kommen. Anstelle den `AttributeConverter` für den richtigen Generic zu nutzen wird ein anderer (der Erste, der genutzt wird?) genutzt.

## Lösung
`@Association` benutzen und Ids direkt benutzen.

### Code
```kotlin
@Table(name = "references_company")  
class ReferenceCompany(  
    override val id: CompanyId,  
    var name: String,  
    var logo: ByteArray,  
    var url: String,  
    //TODO use the Association Type. Last time I tried this mfer tried using the ReferenceEmployeeAssociationConverterInstead  
    @Association// Guess we're doin @Association now  
    @Convert(converter = RequiredFieldIdConverter::class)  
    var requiredField: MutableList<RequiredFieldId>,  
    var wordTemplate: ByteArray?,  
    var powerPointTemplate: ByteArray?,  
): AggregateRoot<ReferenceCompany, CompanyId>
```