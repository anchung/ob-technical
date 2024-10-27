---
{"dg-publish":true,"permalink":"/books/2024/spring-in-action-6th-ed/chapter-1-getting-started-with-spring/","tags":["spring","spring-boot"]}
---

Spring offers a container, often referred to as the *Spring application context* that creates and manages application components. The components, or *beans*, are wired together inside the Spring application context to make a complete application.

The act of wiring beans together is based on a pattern known as *Dependency Injfection (DI)*. A dependency-injected application relies on a separated entity (*container*) to create and maintain all the components (lifecycle). It's done through *constructor arguments* or *property accessor method*

On top of the core container, Spring and a full portfolio of related libraries offer a web framework, persistence options, a security framework, integration with other systems, runtim monitoring, microservice support, a reactive programming model and many more.

```ad-example
title: Wiring example
```java
@Configuration
public class ServiceConfiguration {
@Bean
public InventoryService inventoryService() {
return new InventoryService();
}
@Bean
public ProductService productService() {
return new ProductService(inventoryService());
}
}
```

Automatic configuration has its roots in the Spring techniques known as *autowiring* and *component scanning*. With component scanning, Spring can automatically discover components from an application's classpath and create them as beans in the Spring application context. With autowiring, Spring automatically injects the components with the other beans that they depend on.

*Spring Boot* - autoconfiguration

All `<dependencies>` in pom.xml except for the DevTools dependency have the word *starter* in their artifact ID. Spring Boot starter dependencies are special in that they typically don't have any library code themselves but instead transitively pull in other libraries.
- The bundle file will be significantly smaller and easier to manage
- Thinking of dependencies from the capabilities perspective, rather than the library names
- Free from the burden of worrying about library versions

The *plugin* performs:
- Enable to run application using *Maven*
- Ensure that all dependency libraries are included within the executable JAR file and available on the runtime classpath
- Produce a manifest file in the JAR file that denotes the bootstrap class as the main class
# Annotation
`@SpringBootApplication` is a composite annotation that combines the following three annotations
- `@SpringBootConfiguration` - you can add Java-based Spring Framework configuration to this class if needed
- `@EnableAutoConfiguration` - Enables Spring Boot automatic configuration.
- `@ComponentScan` - Enables component scanning

# Spring Framily
- Core Spring Framework
- Spring Boot
- Spring Data
- Spring Security
- Spring Cloud
- Spring Native
