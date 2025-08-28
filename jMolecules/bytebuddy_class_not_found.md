# ByteBuddy Plugin - `ClassNotFoundException`

## Meta
Status: Analysiert, Dokumentation angepasst
Ticket: https://github.com/xmolecules/jmolecules-integrations/issues/334

## Zusammenfassung

Beim ausführen des `transform` Tasks mit dem `jmolecules-bytebudd-nodep` Plugin, wird eine `ClassNotFoundException: org.hibernate.annotations.EmbeddableInstantiator` geworfen.

Wenn die `jmolecules-bytebuddy` Plugin genutzt wird, wird stattdessen eine `ExceptionInInitializerError: NullPointerException` geworfen.

## Code
```xml
<build>
	<!-- ommited for legibility -->
    <plugins>
	    <!-- ommited for legibility -->
        <plugin>
			<groupId>net.bytebuddy</groupId>
            <artifactId>byte-buddy-maven-plugin</artifactId>
            <version>${bytebuddy.version}</version>
            <executions>
				<execution>
                    <goals>
                        <goal>transform-extended</goal> <!-- Enable the source code transformation -->  
                    </goals>
                </execution>
            </executions>
            <dependencies>
					<dependency>
						<groupId>org.jmolecules.integrations</groupId>
						<artifactId>jmolecules-bytebuddy</artifactId>
						<version>${jmolecules-integrations.version}</version>
					</dependency>
				</dependencies>
		</plugin>  
    </plugins>  
</build>
```

### Stack Trace
```javastacktrace
Failed to execute goal net.bytebuddy:byte-buddy-maven-plugin:1.17.2:transform-extended (default) on project modulith-test: Failed to transform class files in C:\Users\waeckerle\IdeaProjects\modulith-test\target\classes: java.lang.ClassNotFoundException: org.hibernate.annotations.EmbeddableInstantiator -> [Help 1]
org.apache.maven.lifecycle.LifecycleExecutionException: Failed to execute goal net.bytebuddy:byte-buddy-maven-plugin:1.17.2:transform-extended (default) on project modulith-test: Failed to transform class files in C:\Users\waeckerle\IdeaProjects\modulith-test\target\classes
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute2 (MojoExecutor.java:375)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute (MojoExecutor.java:351)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:215)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:171)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:163)
    at org.apache.maven.lifecycle.internal.MojoExecutor.executeForkedExecutions (MojoExecutor.java:508)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute (MojoExecutor.java:345)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:215)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:171)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:163)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:117)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:81)
    at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build (SingleThreadedBuilder.java:56)
    at org.apache.maven.lifecycle.internal.LifecycleStarter.execute (LifecycleStarter.java:128)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:299)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:193)
    at org.apache.maven.DefaultMaven.execute (DefaultMaven.java:106)
    at org.apache.maven.cli.MavenCli.execute (MavenCli.java:963)
    at org.apache.maven.cli.MavenCli.doMain (MavenCli.java:296)
    at org.apache.maven.cli.MavenCli.main (MavenCli.java:199)
    at jdk.internal.reflect.NativeMethodAccessorImpl.invoke0 (Native Method)
    at jdk.internal.reflect.NativeMethodAccessorImpl.invoke (NativeMethodAccessorImpl.java:77)
    at jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke (DelegatingMethodAccessorImpl.java:43)
    at java.lang.reflect.Method.invoke (Method.java:568)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced (Launcher.java:282)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launch (Launcher.java:225)
    at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode (Launcher.java:406)
    at org.codehaus.plexus.classworlds.launcher.Launcher.main (Launcher.java:347)
Caused by: org.apache.maven.plugin.MojoExecutionException: Failed to transform class files in C:\Users\waeckerle\IdeaProjects\modulith-test\target\classes       
    at net.bytebuddy.build.maven.ByteBuddyMojo.transform (ByteBuddyMojo.java:447)
    at net.bytebuddy.build.maven.ByteBuddyMojo$ForLifecycleTypes.apply (ByteBuddyMojo.java:605)
    at net.bytebuddy.build.maven.ByteBuddyMojo.execute (ByteBuddyMojo.java:310)
    at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo (DefaultBuildPluginManager.java:137)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute2 (MojoExecutor.java:370)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute (MojoExecutor.java:351)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:215)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:171)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:163)
    at org.apache.maven.lifecycle.internal.MojoExecutor.executeForkedExecutions (MojoExecutor.java:508)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute (MojoExecutor.java:345)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:215)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:171)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:163)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:117)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:81)
    at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build (SingleThreadedBuilder.java:56)
    at org.apache.maven.lifecycle.internal.LifecycleStarter.execute (LifecycleStarter.java:128)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:299)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:193)
    at org.apache.maven.DefaultMaven.execute (DefaultMaven.java:106)
    at org.apache.maven.cli.MavenCli.execute (MavenCli.java:963)
    at org.apache.maven.cli.MavenCli.doMain (MavenCli.java:296)
    at org.apache.maven.cli.MavenCli.main (MavenCli.java:199)
    at jdk.internal.reflect.NativeMethodAccessorImpl.invoke0 (Native Method)
    at jdk.internal.reflect.NativeMethodAccessorImpl.invoke (NativeMethodAccessorImpl.java:77)
    at jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke (DelegatingMethodAccessorImpl.java:43)
    at java.lang.reflect.Method.invoke (Method.java:568)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced (Launcher.java:282)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launch (Launcher.java:225)
    at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode (Launcher.java:406)
    at org.codehaus.plexus.classworlds.launcher.Launcher.main (Launcher.java:347)
Caused by: java.lang.IllegalStateException: java.lang.ClassNotFoundException: org.hibernate.annotations.EmbeddableInstantiator
    at org.jmolecules.bytebuddy.Jpa.loadClass (Jpa.java:162)
    at org.jmolecules.bytebuddy.Jpa.getType (Jpa.java:107)
    at org.jmolecules.bytebuddy.JMoleculesJpaPlugin.init (JMoleculesJpaPlugin.java:75)
    at org.jmolecules.bytebuddy.JMoleculesJpaPlugin.<init> (JMoleculesJpaPlugin.java:63)
    at org.jmolecules.bytebuddy.JMoleculesPlugin.lambda$jpaPlugin$7 (JMoleculesPlugin.java:124)
    at java.util.Optional.map (Optional.java:260)
    at org.jmolecules.bytebuddy.JMoleculesPlugin.jpaPlugin (JMoleculesPlugin.java:124)
    at org.jmolecules.bytebuddy.JMoleculesPlugin.lambda$onPreprocess$1 (JMoleculesPlugin.java:63)
    at java.util.HashMap.computeIfAbsent (HashMap.java:1220)
    at org.jmolecules.bytebuddy.JMoleculesPlugin.onPreprocess (JMoleculesPlugin.java:54)
    at net.bytebuddy.build.Plugin$Engine$Default$Preprocessor.call (Plugin.java:5255)
    at net.bytebuddy.build.Plugin$Engine$Default$Preprocessor.call (Plugin.java:5170)
    at net.bytebuddy.build.Plugin$Engine$Dispatcher$ForSerialTransformation.accept (Plugin.java:4202)
    at net.bytebuddy.build.Plugin$Engine$Default.apply (Plugin.java:5048)
    at net.bytebuddy.build.maven.ByteBuddyMojo.transform (ByteBuddyMojo.java:445)
    at net.bytebuddy.build.maven.ByteBuddyMojo$ForLifecycleTypes.apply (ByteBuddyMojo.java:605)
    at net.bytebuddy.build.maven.ByteBuddyMojo.execute (ByteBuddyMojo.java:310)
    at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo (DefaultBuildPluginManager.java:137)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute2 (MojoExecutor.java:370)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute (MojoExecutor.java:351)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:215)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:171)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:163)
    at org.apache.maven.lifecycle.internal.MojoExecutor.executeForkedExecutions (MojoExecutor.java:508)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute (MojoExecutor.java:345)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:215)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:171)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:163)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:117)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:81)
    at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build (SingleThreadedBuilder.java:56)
    at org.apache.maven.lifecycle.internal.LifecycleStarter.execute (LifecycleStarter.java:128)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:299)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:193)
    at org.apache.maven.DefaultMaven.execute (DefaultMaven.java:106)
    at org.apache.maven.cli.MavenCli.execute (MavenCli.java:963)
    at org.apache.maven.cli.MavenCli.doMain (MavenCli.java:296)
    at org.apache.maven.cli.MavenCli.main (MavenCli.java:199)
    at jdk.internal.reflect.NativeMethodAccessorImpl.invoke0 (Native Method)
    at jdk.internal.reflect.NativeMethodAccessorImpl.invoke (NativeMethodAccessorImpl.java:77)
    at jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke (DelegatingMethodAccessorImpl.java:43)
    at java.lang.reflect.Method.invoke (Method.java:568)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced (Launcher.java:282)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launch (Launcher.java:225)
    at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode (Launcher.java:406)
    at org.codehaus.plexus.classworlds.launcher.Launcher.main (Launcher.java:347)
Caused by: java.lang.ClassNotFoundException: org.hibernate.annotations.EmbeddableInstantiator
    at org.codehaus.plexus.classworlds.strategy.SelfFirstStrategy.loadClass (SelfFirstStrategy.java:50)
    at org.codehaus.plexus.classworlds.realm.ClassRealm.unsynchronizedLoadClass (ClassRealm.java:271)
    at org.codehaus.plexus.classworlds.realm.ClassRealm.loadClass (ClassRealm.java:247)
    at org.codehaus.plexus.classworlds.realm.ClassRealm.loadClass (ClassRealm.java:239)
    at java.lang.Class.forName0 (Native Method)
    at java.lang.Class.forName (Class.java:467)
    at org.jmolecules.bytebuddy.Jpa.loadClass (Jpa.java:160)
    at org.jmolecules.bytebuddy.Jpa.getType (Jpa.java:107)
    at org.jmolecules.bytebuddy.JMoleculesJpaPlugin.init (JMoleculesJpaPlugin.java:75)
    at org.jmolecules.bytebuddy.JMoleculesJpaPlugin.<init> (JMoleculesJpaPlugin.java:63)
    at org.jmolecules.bytebuddy.JMoleculesPlugin.lambda$jpaPlugin$7 (JMoleculesPlugin.java:124)
    at java.util.Optional.map (Optional.java:260)
    at org.jmolecules.bytebuddy.JMoleculesPlugin.jpaPlugin (JMoleculesPlugin.java:124)
    at org.jmolecules.bytebuddy.JMoleculesPlugin.lambda$onPreprocess$1 (JMoleculesPlugin.java:63)
    at java.util.HashMap.computeIfAbsent (HashMap.java:1220)
    at org.jmolecules.bytebuddy.JMoleculesPlugin.onPreprocess (JMoleculesPlugin.java:54)
    at net.bytebuddy.build.Plugin$Engine$Default$Preprocessor.call (Plugin.java:5255)
    at net.bytebuddy.build.Plugin$Engine$Default$Preprocessor.call (Plugin.java:5170)
    at net.bytebuddy.build.Plugin$Engine$Dispatcher$ForSerialTransformation.accept (Plugin.java:4202)
    at net.bytebuddy.build.Plugin$Engine$Default.apply (Plugin.java:5048)
    at net.bytebuddy.build.maven.ByteBuddyMojo.transform (ByteBuddyMojo.java:445)
    at net.bytebuddy.build.maven.ByteBuddyMojo$ForLifecycleTypes.apply (ByteBuddyMojo.java:605)
    at net.bytebuddy.build.maven.ByteBuddyMojo.execute (ByteBuddyMojo.java:310)
    at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo (DefaultBuildPluginManager.java:137)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute2 (MojoExecutor.java:370)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute (MojoExecutor.java:351)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:215)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:171)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:163)
    at org.apache.maven.lifecycle.internal.MojoExecutor.executeForkedExecutions (MojoExecutor.java:508)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute (MojoExecutor.java:345)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:215)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:171)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:163)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:117)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:81)
    at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build (SingleThreadedBuilder.java:56)
    at org.apache.maven.lifecycle.internal.LifecycleStarter.execute (LifecycleStarter.java:128)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:299)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:193)
    at org.apache.maven.DefaultMaven.execute (DefaultMaven.java:106)
    at org.apache.maven.cli.MavenCli.execute (MavenCli.java:963)
    at org.apache.maven.cli.MavenCli.doMain (MavenCli.java:296)
    at org.apache.maven.cli.MavenCli.main (MavenCli.java:199)
    at jdk.internal.reflect.NativeMethodAccessorImpl.invoke0 (Native Method)
    at jdk.internal.reflect.NativeMethodAccessorImpl.invoke (NativeMethodAccessorImpl.java:77)
    at jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke (DelegatingMethodAccessorImpl.java:43)
    at java.lang.reflect.Method.invoke (Method.java:568)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced (Launcher.java:282)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launch (Launcher.java:225)
    at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode (Launcher.java:406)
    at org.codehaus.plexus.classworlds.launcher.Launcher.main (Launcher.java:347)

```

## Ursache
Falsche Konfiguration der ByteBuddy Plugin?

## Lösung
Anstelle, dass wie in viele Beispielen der ByteBuddy Plugin das jMolecules ByteBuddy Plugin, als Dependency, hinzugefügt wird. Soll die Dependency nicht hinzugefügt werden und die `classPathDiscovery` Config auf `true` gesetzt werden, wie in den [offiziellen Beispielen](https://github.com/xmolecules/jmolecules-examples).

### Code
```xml
<build>
	<!-- ommited for legibility -->
    <plugins>
	    <!-- ommited for legibility -->
        <plugin>
			<groupId>net.bytebuddy</groupId>  
            <artifactId>byte-buddy-maven-plugin</artifactId>  
            <version>${bytebuddy.version}</version>  
            <executions>
				<execution>  
                    <goals>  
                        <goal>transform-extended</goal> <!-- Enable the source code transformation -->  
                    </goals>  
                </execution>  
            </executions>  
            <configuration>
				<classPathDiscovery>true</classPathDiscovery>  
                <initialization>  
                    <validated>false</validated>
				</initialization>
			</configuration>
		</plugin>  
    </plugins>  
</build>
```