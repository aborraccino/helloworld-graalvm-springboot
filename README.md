# HelloWorld SpringBoot with GraalVM
This is a simple Spring Boot Hello World service with GraalVm support.

This uses:
- Java 21 (21.0.2-graal)
- Gradle
- Spring Boot MVC

## GraalVM Native Support

This project has been configured to let you generate either a lightweight container or a native executable
adding the following plugin in the build.gradle file:
```
id 'org.graalvm.buildtools.native' version '0.9.28' 
```

It is also possible to run your tests in a native image.

### Requirements
- GraalVM 21
    - `` sdk install java 21.0.2-graal ``
- Docker if you want to containerise it

### Lightweight Container with Cloud Native Buildpacks
If you're already familiar with Spring Boot container images support, this is the easiest way to get started.
Docker should be installed and configured on your machine prior to creating the image.

To create the image, run the following goal:

```
$ ./gradlew bootBuildImage
```

Then, you can run the app like any other container:

```
$ docker run --rm -p 8080:8080 helloworld-graalvm-springboot:0.0.1-SNAPSHOT
```

### Executable with Native Build Tools
Use this option if you want to explore more options such as running your tests in a native image.
The GraalVM `native-image` compiler should be installed and configured on your machine.

NOTE: GraalVM 22.3+ is required.

To create the executable, run the following goal:

```
$ ./gradlew nativeCompile
```

Then, you can run the app as follows:
```
$ build/native/nativeCompile/helloworld-graalvm-springboot
```

You can also run your existing tests suite in a native image.
This is an efficient way to validate the compatibility of your application.

To run your existing tests in a native image, run the following goal:

```
$ ./gradlew nativeTest
```

### Startup time comparison

With a normal Jar:
```shell
% java -jar build/libs/helloworld-graalvm-springboot-0.0.1-SNAPSHOT.jar
2024-02-02T12:38:57.449Z  INFO 41451 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port 8080 (http) with context path ''
2024-02-02T12:38:57.473Z  INFO 41451 --- [           main] .w.t.g.h.HelloWorldSpringbootApplication : Started HelloWorldSpringbootApplication in 2.537 seconds (process running for 3.308)
```

With a native image:
```shell
% ./build/native/nativeCompile/helloworld-graalvm-springboot
2024-02-02T12:40:33.517Z  INFO 41610 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port 8080 (http) with context path ''
2024-02-02T12:40:33.518Z  INFO 41610 --- [           main] .w.t.g.h.HelloWorldSpringbootApplication : Started HelloWorldSpringbootApplication in 0.077 seconds (process running for 0.1)
```

**Result: 2.537 (jar) vs 0.077 (native) seconds**

### Benefits
- Performance
  - Startup time significantly improved
  - No need to warmup containers
  - Low memory consumption
- Size
  - Smaller images
  - Smaller container
- Development
  - Easy to get started
  - Gradle plugin

### Drawbacks
- Compatibility Issues
  - Spring dynamic beans
  - Spring profiles
  - Many other features
- Longer build times
  - ~5m to build a Hello World that would otherwise take ~1m
  - No noticeable performance difference post warmup

### Real World use cases
- Startup time is critical
- You have few third-party libraries
- You want to reduce the cost of cloud infrastructure
- You need a lightweight application (K8s Jobs, AWS Lambda)

### Reference Documentation

- [GraalVM official doc](https://www.graalvm.org/latest/docs/)
- [Spring Boot GraalVM support Doc](https://docs.spring.io/spring-boot/docs/current/reference/html/native-image.html)

For further reference, please consider the following sections:

* [Official Gradle documentation](https://docs.gradle.org)
* [Spring Boot Gradle Plugin Reference Guide](https://docs.spring.io/spring-boot/docs/3.2.2/gradle-plugin/reference/html/)
* [Create an OCI image](https://docs.spring.io/spring-boot/docs/3.2.2/gradle-plugin/reference/html/#build-image)
* [GraalVM Native Image Support](https://docs.spring.io/spring-boot/docs/3.2.2/reference/html/native-image.html#native-image)
* [Spring Web](https://docs.spring.io/spring-boot/docs/3.2.2/reference/htmlsingle/index.html#web)

### Guides
The following guides illustrate how to use some features concretely:

* [Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/)
* [Serving Web Content with Spring MVC](https://spring.io/guides/gs/serving-web-content/)
* [Building REST services with Spring](https://spring.io/guides/tutorials/rest/)

### Additional Links
These additional references should also help you:

* [Gradle Build Scans – insights for your project's build](https://scans.gradle.com#gradle)
* [Configure AOT settings in Build Plugin](https://docs.spring.io/spring-boot/docs/3.2.2/gradle-plugin/reference/htmlsingle/#aot)


