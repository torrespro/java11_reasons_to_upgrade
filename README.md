# java11_reasons_to_upgrade

## Background

Oracle’s JDK 8 support roadmap [https://www.oracle.com/java/technologies/java-se-support-roadmap.html]

From Java 11, Oracle has changed the license of their JDK, so instead of having a single JDK build which can be used either commercially (i.e. with paid support) or for free (which many of us were doing), they now have two different JDK builds:

Oracle’s JDK (commercial) – you can use this in development and testing for free, but if you use it in production you have to pay for it
Oracle’s OpenJDK (open source) – you can use this for free in any environment, like any open source library
Note that since Java 11, Oracle’s commercial JDK and Oracle’s OpenJDK builds are functionally the same, so we should be able to run our applications on either without having to make any changes or losing any features.

## Support and Updates

There’s an important difference between these two builds though – if you’re using Oracle’s commercial JDK, you’ll get updates and support.  If you’re using Oracle’s OpenJDK build, Oracle won’t be providing updates to past versions. What this means is now Java 11 is out Oracle will no longer be updating their OpenJDK builds for 10 or 9. So, if you’re using Oracle’s OpenJDK build, you should be prepared to update to each new version of Java as it comes out (or run an older version that won’t get updates).

Having said that, Oracle is not the only vendor in this game.  For years, we’ve been used to using their JDK for free and so it has generally been our default, but there are other vendors who provide JDKs, and they have different support models (free and paid for), and different attitudes towards providing updates for different versions of Java.  Some of these other vendors, for example, may continue to provide updates and/or support for Java 9 (which Oracle will not).

Stephen Colebourne has written a [post summarizing the different JDK builds](https://blog.joda.org/2018/09/time-to-look-beyond-oracles-jdk.html?m=1) from the different vendors, and explaining how (and why) all these vendors provide different builds based off the same code (i.e. OpenJDK).

If we don’t want to use either of the Oracle JDK builds (for example if their support or updates policies doesn’t suit you), we can investigate what [Azul](https://www.azul.com/products/zulu-enterprise/), [IBM](https://developer.ibm.com/javasdk/support/lifecycle/), [Red Hat](https://access.redhat.com/articles/1299013), and the community-led [AdoptOpenJDK](https://adoptopenjdk.net/) have to offer instead.

## Relevant data

- ~~Oracle’s JDK 8 will no longer receive public updates after January 2019. If you want to receive updates to Java 8, you may need to pay Oracle or to find another JDK build.~~ Recently changed: https://blogs.oracle.com/java-platform-group/extension-of-oracle-java-se-8-public-updates-and-java-web-start-support
- Java 11 is a Long Term Support (LTS) version that provides long-term support for 3 years since Java 8.

### New functions added in Java 11 are as follows.
- Nest based access control function "Nestmates"
- Dynamic class file constant "condy"
- New garbage collector "ZGC"
- Flight Recorder
- New standard HTTP library · TLS 1.3 support · Local variable syntax for lambda parameters

### Deleted functions are as follows:

- Java EE module · CORBA module · Web Start
- Applets
- JavaFX module

## Impact:
### Upgrade the code
There are basically three incremental phases to fully migrate to Java 11:

#### Run an existing Java application with JDK 11.
This is a really simple step, the application (jars) created with earlier Java versions can run on JDK 11 without major issues, except if you depends on Java EE or CORBA modules which were removed from JDK in [JEP-320](http://openjdk.java.net/jeps/320).

In the case of missing classes, you may need to explicitly add java.activation, java.transaction and java.xml.bind dependencies, and in case of class file errors you will need update Java bytecode enhancement libraries like ASM, bytebuddy, javassist or cglib.

#### Compile the application with Java 11.
*Why upgrade source to Java 11?*

- Local variable type inference (var keyword).
- New native unmodifiable collections APIs.
- New reactive streams APIs.
- Improved streams/predicate/optional APIs.
- Improved system process API.
- Improved files API.
- Support for HTTP/2.
- Standard Java Async HTTP client.
- Multi-release JARs.

#### Modularize the application to use [Module System](http://openjdk.java.net/projects/jigsaw/spec/).
*Why migrate to Module System?*

- Reliable configuration — to replace the brittle, error-prone class-path mechanism with a means for program components to declare explicit dependences upon one another.
- Strong encapsulation — to allow a component to declare which of its public types are accessible to other components, and which are not.
- Create a minimal JRE image for your application.
- Decrease application memory footprint.
- Optimize application startup time.

### Performance improvements

Java 11 brings additional improvements. **On average, it is 4.5% faster when using Parallel GC and 16.1% faster with G1 GC**. Despite the significant improvement for G1 GC, Parallel GC is still faster for most data sets in this benchmark.

![alt text](https://www.optaplanner.org/blog/2019/01/17/Java8VsJava11usingParallelGC.svg)

![alt text](https://www.optaplanner.org/blog/2019/01/17/Java8VsJava11usingG1GC.svg)

Source: optaplanner.org/blog/2019/01/17/HowMuchFasterIsJava11.html

### Other frameworks impact
#### Spring Support for JDK 11
During the SpringOne platform, support from the Spring Framework 5.1 with Java 11 was announced. Spring Framework 4.3 will support up to Java 8, 5.0 will support Java 9, and 5.1 will officially support Java 11.

If you are using Spring Boot, Java 11 is supported as of SpringBoot 2.1.X. The plan is to officially support Java 12 as of Spring Boot 2.2.

Also, if you are using Pivotal or Cloud Foundry for PaaS solutions, there are extra measures you have to take care of for Spring Boot 2.X.X versions to deploy.

Spring Boot 2.1 uses Spring Framework 5.1, with Spring being updated to the stable releases of all the dependencies. Hence, you can officially use the Spring Starter with Spring Boot 2.1.1 and compile using Java 11.

Additionally, if you are using Maven to build your source code, you need to upgrade the Maven Compiler plugin to > 3.5 version.

Service SDK 8.0 uses Spring Boot 2.1 and Spring 5.1

#### Quarkus Support for JDK 11
They plan to drop Java 8 support in a few releases, probably around 1.6. Source: https://quarkus.io/blog/quarkus-1-3-1-final-released/

### Application servers impact

| Application Server  | Version  | Supported Java Version  |  
|----------------------------|----------|------|
| Apache Tomcat              | 8.5      | 1.8+ | 
| JBoss EAP                  | 7.1.0    | 1.8  |  
| IBM WebSphere Liberty Core | 17.0.0   | 1.8  |  


- JBoss EAP 7.2 supports JDK 11, see https://access.redhat.com/articles/2026253

- Liberty is supported by any compliant Java™ SE 7, or Java SE 8 runtime environment (JRE) or Java SDK, subject to the minimum supported levels shown for the following specific implementations.
As of Liberty 19.0.0.1, any distribution of Java SE 11 may be used. The minimum supported level is Java version 11.0.2. Note that significant changes in the Java SE platform occurred between Java SE 8 and Java SE 11, so zero-migration is not guaranteed.
