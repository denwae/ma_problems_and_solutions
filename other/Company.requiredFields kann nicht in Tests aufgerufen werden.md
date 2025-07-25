# `Company.requiredFields` kann nicht in Tests aufgerufen werden
Wenn die `test-compile` Task der Kotlin Maven Plugin aufgerufen wird, wird eine `java.lang.StringIndexOutOfBoundsException` geworfen.

## Code

```kotlin
assertAll(  
	{ assertEquals(updatedCompany.name, company.name) },  
	{ assertTrue(updatedCompany.logo.contentEquals(company.logo)) },  
	{ assertEquals(updatedCompany.url, company.url) },  
	{ assertTrue(updatedCompany.wordTemplate?.file.contentEquals(company.wordTemplate?.file)) },  
	{ assertTrue(updatedCompany.powerPointTemplate?.file.contentEquals(company.powerPointTemplate?.file)) },  
	{ assertTrue(updatedCompany.requiredFields.containsAll(company.requiredFields.map { it.id.id })) }  
)
```

### Stack Trace
```javastacktrace
java.lang.StringIndexOutOfBoundsException: Range [1, 0) out of bounds for length 1
        at java.base/jdk.internal.util.Preconditions$1.apply(Preconditions.java:55)
        at java.base/jdk.internal.util.Preconditions$1.apply(Preconditions.java:52)
        at java.base/jdk.internal.util.Preconditions$4.apply(Preconditions.java:213)
        at java.base/jdk.internal.util.Preconditions$4.apply(Preconditions.java:210)
        at java.base/jdk.internal.util.Preconditions.outOfBounds(Preconditions.java:98)
        at java.base/jdk.internal.util.Preconditions.outOfBoundsCheckFromToIndex(Preconditions.java:112)
        at java.base/jdk.internal.util.Preconditions.checkFromToIndex(Preconditions.java:349)
        at java.base/java.lang.String.checkBoundsBeginEnd(String.java:4865)
        at java.base/java.lang.String.substring(String.java:2834)
        at org.jetbrains.kotlin.load.kotlin.FileBasedKotlinClass.resolveNameByDesc(FileBasedKotlinClass.java:298)
        at org.jetbrains.kotlin.load.kotlin.FileBasedKotlinClass.resolveKotlinNameByType(FileBasedKotlinClass.java:316)
        at org.jetbrains.kotlin.load.kotlin.FileBasedKotlinClass.access$200(FileBasedKotlinClass.java:29)
        at org.jetbrains.kotlin.load.kotlin.FileBasedKotlinClass$3.visit(FileBasedKotlinClass.java:169)
        at org.jetbrains.org.objectweb.asm.ClassReader.readElementValue(ClassReader.java:3060)
        at org.jetbrains.org.objectweb.asm.ClassReader.readElementValues(ClassReader.java:2972)
        at org.jetbrains.org.objectweb.asm.ClassReader.readField(ClassReader.java:1120)
        at org.jetbrains.org.objectweb.asm.ClassReader.accept(ClassReader.java:707)
        at org.jetbrains.org.objectweb.asm.ClassReader.accept(ClassReader.java:395)
        at org.jetbrains.kotlin.load.kotlin.FileBasedKotlinClass.visitMembers(FileBasedKotlinClass.java:230)
        at org.jetbrains.kotlin.load.kotlin.AbstractBinaryClassAnnotationAndConstantLoader.loadAnnotationsAndInitializers(AbstractBinaryClassAnnotationAndConstantLoader.kt:98)       
        at org.jetbrains.kotlin.load.kotlin.AbstractBinaryClassAnnotationAndConstantLoader.access$loadAnnotationsAndInitializers(AbstractBinaryClassAnnotationAndConstantLoader.kt:24)
        at org.jetbrains.kotlin.load.kotlin.AbstractBinaryClassAnnotationAndConstantLoader$storage$1.invoke(AbstractBinaryClassAnnotationAndConstantLoader.kt:32)
        at org.jetbrains.kotlin.load.kotlin.AbstractBinaryClassAnnotationAndConstantLoader$storage$1.invoke(AbstractBinaryClassAnnotationAndConstantLoader.kt:31)
        at org.jetbrains.kotlin.storage.LockBasedStorageManager$MapBasedMemoizedFunction.invoke(LockBasedStorageManager.java:578)
        at org.jetbrains.kotlin.storage.LockBasedStorageManager$MapBasedMemoizedFunctionToNotNull.invoke(LockBasedStorageManager.java:681)
        at org.jetbrains.kotlin.load.kotlin.AbstractBinaryClassAnnotationAndConstantLoader.getAnnotationsContainer(AbstractBinaryClassAnnotationAndConstantLoader.kt:35)
        at org.jetbrains.kotlin.load.kotlin.AbstractBinaryClassAnnotationAndConstantLoader.getAnnotationsContainer(AbstractBinaryClassAnnotationAndConstantLoader.kt:24)
        at org.jetbrains.kotlin.load.kotlin.AbstractBinaryClassAnnotationLoader.findClassAndLoadMemberAnnotations(AbstractBinaryClassAnnotationLoader.kt:149)
        at org.jetbrains.kotlin.load.kotlin.AbstractBinaryClassAnnotationLoader.loadPropertyAnnotations(AbstractBinaryClassAnnotationLoader.kt:113)
        at org.jetbrains.kotlin.load.kotlin.AbstractBinaryClassAnnotationLoader.loadPropertyBackingFieldAnnotations(AbstractBinaryClassAnnotationLoader.kt:83)
        at org.jetbrains.kotlin.serialization.deserialization.MemberDeserializer$getPropertyFieldAnnotations$1.invoke(MemberDeserializer.kt:306)
        at org.jetbrains.kotlin.serialization.deserialization.MemberDeserializer$getPropertyFieldAnnotations$1.invoke(MemberDeserializer.kt:301)
        at org.jetbrains.kotlin.storage.LockBasedStorageManager$LockBasedLazyValue.invoke(LockBasedStorageManager.java:408)
        at org.jetbrains.kotlin.storage.LockBasedStorageManager$LockBasedNotNullLazyValue.invoke(LockBasedStorageManager.java:527)
        at org.jetbrains.kotlin.storage.StorageKt.getValue(storage.kt:42)
        at org.jetbrains.kotlin.serialization.deserialization.descriptors.DeserializedAnnotations.getAnnotations(DeserializedAnnotations.kt:28)
        at org.jetbrains.kotlin.serialization.deserialization.descriptors.DeserializedAnnotations.iterator(DeserializedAnnotations.kt:32)
        at org.jetbrains.kotlin.descriptors.annotations.Annotations$DefaultImpls.findAnnotation(Annotations.kt:124)
        at org.jetbrains.kotlin.serialization.deserialization.descriptors.DeserializedAnnotations.findAnnotation(DeserializedAnnotations.kt:24)
        at org.jetbrains.kotlin.resolve.jvm.annotations.JvmAnnotationUtilKt.findJvmFieldAnnotation(jvmAnnotationUtil.kt:52)
        at org.jetbrains.kotlin.resolve.jvm.annotations.JvmAnnotationUtilKt.hasJvmFieldAnnotation(jvmAnnotationUtil.kt:55)
        at org.jetbrains.kotlin.backend.jvm.JvmGeneratorExtensionsImpl.isPropertyWithPlatformField(JvmGeneratorExtensionsImpl.kt:114)
        at org.jetbrains.kotlin.ir.declarations.lazy.IrLazyProperty.<init>(IrLazyProperty.kt:51)
        at org.jetbrains.kotlin.ir.util.DeclarationStubGenerator$generatePropertyStub$1.invoke(DeclarationStubGenerator.kt:143)
        at org.jetbrains.kotlin.ir.util.DeclarationStubGenerator$generatePropertyStub$1.invoke(DeclarationStubGenerator.kt:142)
        at org.jetbrains.kotlin.ir.util.SymbolTableExtension.declareProperty(SymbolTableExtension.kt:1121)
        at org.jetbrains.kotlin.ir.util.DeclarationStubGenerator.generatePropertyStub(DeclarationStubGenerator.kt:142)
        at org.jetbrains.kotlin.ir.util.DeclarationStubGenerator.generateFunctionStub(DeclarationStubGenerator.kt:184)
        at org.jetbrains.kotlin.ir.util.DeclarationStubGenerator.generateFunctionStub$default(DeclarationStubGenerator.kt:177)
        at org.jetbrains.kotlin.ir.util.DeclarationStubGenerator.generateMemberStub(DeclarationStubGenerator.kt:112)
        at org.jetbrains.kotlin.ir.backend.jvm.serialization.JvmIrLinker$MetadataJVMModuleDeserializer.declareIrSymbol(JvmIrLinker.kt:170)
        at org.jetbrains.kotlin.backend.common.serialization.IrModuleDeserializerWithBuiltIns.declareIrSymbol(IrModuleDeserializer.kt:212)
        at org.jetbrains.kotlin.backend.common.serialization.KotlinIrLinker.findDeserializedDeclarationForSymbol(KotlinIrLinker.kt:119)
        at org.jetbrains.kotlin.backend.common.serialization.KotlinIrLinker.deserializeOrResolveDeclaration(KotlinIrLinker.kt:158)
        at org.jetbrains.kotlin.ir.backend.jvm.serialization.JvmIrLinker.getDeclaration(JvmIrLinker.kt:104)
        at org.jetbrains.kotlin.ir.util.ExternalDependenciesGenerator.generateUnboundSymbolsAsDependencies(ExternalDependenciesGenerator.kt:39)
        at org.jetbrains.kotlin.psi2ir.generators.ModuleGenerator.generateUnboundSymbolsAsDependencies(ModuleGenerator.kt:58)
        at org.jetbrains.kotlin.psi2ir.Psi2IrTranslator.generateModuleFragment(Psi2IrTranslator.kt:102)
        at org.jetbrains.kotlin.backend.jvm.JvmIrCodegenFactory.convertToIr(JvmIrCodegenFactory.kt:255)
        at org.jetbrains.kotlin.backend.jvm.JvmIrCodegenFactory.convertToIr(JvmIrCodegenFactory.kt:59)
        at org.jetbrains.kotlin.cli.jvm.compiler.KotlinToJVMBytecodeCompiler.convertToIr(KotlinToJVMBytecodeCompiler.kt:224)
        at org.jetbrains.kotlin.cli.jvm.compiler.KotlinToJVMBytecodeCompiler.compileModules$cli(KotlinToJVMBytecodeCompiler.kt:101)
        at org.jetbrains.kotlin.cli.jvm.compiler.KotlinToJVMBytecodeCompiler.compileModules$cli$default(KotlinToJVMBytecodeCompiler.kt:43)
        at org.jetbrains.kotlin.cli.jvm.K2JVMCompiler.doExecute(K2JVMCompiler.kt:165)
        at org.jetbrains.kotlin.cli.jvm.K2JVMCompiler.doExecute(K2JVMCompiler.kt:50)
        at org.jetbrains.kotlin.cli.common.CLICompiler.execImpl(CLICompiler.kt:104)
        at org.jetbrains.kotlin.cli.common.CLICompiler.execImpl(CLICompiler.kt:48)
        at org.jetbrains.kotlin.cli.common.CLITool.exec(CLITool.kt:101)
        at org.jetbrains.kotlin.maven.KotlinCompileMojoBase.execCompiler(KotlinCompileMojoBase.java:239)
        at org.jetbrains.kotlin.maven.K2JVMCompileMojo.execCompiler(K2JVMCompileMojo.java:240)
        at org.jetbrains.kotlin.maven.K2JVMCompileMojo.execCompiler(K2JVMCompileMojo.java:56)
        at org.jetbrains.kotlin.maven.KotlinCompileMojoBase.execute(KotlinCompileMojoBase.java:220)
        at org.jetbrains.kotlin.maven.K2JVMCompileMojo.execute(K2JVMCompileMojo.java:225)
        at org.jetbrains.kotlin.maven.KotlinTestCompileMojo.execute(KotlinTestCompileMojo.java:84)
        at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo(DefaultBuildPluginManager.java:137)
        at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute2(MojoExecutor.java:370)
        at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute(MojoExecutor.java:351)
        at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:215)
        at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:171)
        at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:163)
        at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:117)
        at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:81)
        at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build(SingleThreadedBuilder.java:56)
        at org.apache.maven.lifecycle.internal.LifecycleStarter.execute(LifecycleStarter.java:128)
        at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:299)
        at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:193)
        at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:106)
        at org.apache.maven.cli.MavenCli.execute(MavenCli.java:963)
        at org.apache.maven.cli.MavenCli.doMain(MavenCli.java:296)
        at org.apache.maven.cli.MavenCli.main(MavenCli.java:199)
        at java.base/jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:103)
        at java.base/java.lang.reflect.Method.invoke(Method.java:580)
        at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced(Launcher.java:282)
        at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:225)
        at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:406)
        at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:347)

```

## Ursache
Unbekannt

## Lösung
Keine richtige Lösung vorhanden: Einfach nicht `requiredFields` in Tests aufrufen.

### Code
```kotlin
assertAll(  
	{ assertEquals(updatedCompany.name, company.name) },  
	{ assertTrue(updatedCompany.logo.contentEquals(company.logo)) },  
	{ assertEquals(updatedCompany.url, company.url) },  
	{ assertTrue(updatedCompany.wordTemplate?.file.contentEquals(company.wordTemplate?.file)) },  
	{ assertTrue(updatedCompany.powerPointTemplate?.file.contentEquals(company.powerPointTemplate?.file)) },
)
```