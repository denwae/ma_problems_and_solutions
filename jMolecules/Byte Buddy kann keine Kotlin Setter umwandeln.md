# ByteBuddy kann keine Kotlin Setter umwandeln
Beim ausführen des `transform` Tasks der ByteBuddy Plugin wird ein Fehler geworfen, weil Setter nicht umgewandelt werden können. Kotlin scheint die Setter als `set-?` zu benennen.

## Code
```xml
<plugin>  
	<groupId>net.bytebuddy</groupId>  
	<artifactId>byte-buddy-maven-plugin</artifactId>  
	<version>${bytebuddy.version}</version>  
	<executions>
		<execution>  
			<goals>  
				<goal>transform-extended</goal>  
			</goals>  
		</execution>  
	</executions>  
	<configuration>                    
		<classPathDiscovery>true</classPathDiscovery>   
	</configuration>  
</plugin>
```

### Stack Trace
```log
[ERROR] Failed to execute goal net.bytebuddy:byte-buddy-maven-plugin:1.17.2:transform-extended (default) on project spring-jmolecules-kotlin-demo: Failed to transform class files in C:\Users\waeckerle\IdeaProjects\spring-jmolecules-kotlin-demo\target\classes: Failed to transform class de.adesso.springjmoleculeskotlindemo.companies.Company: [java.lang.IllegalStateException: Illegal parameter name of java.lang.String <set-?> for public final void de.adesso.springjmoleculeskotlindemo.companies.Company.setName(java.lang.String)] -> [Help 1]

```

## Ursache
Das Kotlin und ByteBuddy Team nutzen unterschiedliche Ansätze, um die kompatibiltät mit der JVM zu bestimmen. Das Kotlin Team nutzt die JVMS 8 Spec und [verweist, dass `set-?` laut Spec ein zulässiger Name ist](https://youtrack.jetbrains.com/issue/KT-38547/Generated-setter-parameter-name-has-invalid-symbols-with-javap#focus=Comments-27-4122309.0-0). Das ByteBuddy Team nutzt [die Java API, um die JVM Kompatibilität zu testen](https://github.com/raphw/byte-buddy/issues/843#issuecomment-623182456), wonach `-` (Bindestriche) nicht zulässig sind.
## Lösung

Validation für die [ByteBuddy Plugin ausschalten](https://github.com/raphw/byte-buddy/issues/843#issuecomment-1295765507).

## Code
```xml
<plugin>  
    <groupId>net.bytebuddy</groupId>  
    <artifactId>byte-buddy-maven-plugin</artifactId>  
    <version>${bytebuddy.version}</version>  
    <executions>
		<execution>  
            <goals>  
                <goal>transform-extended</goal>  
            </goals>  
        </execution>  
    </executions>  
    <configuration>        
	    <classPathDiscovery>true</classPathDiscovery>  
        <initialization>            
	        <validated>false</validated>  
        </initialization>  
    </configuration>  
</plugin>intln("Hello World!)
```