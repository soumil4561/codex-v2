---
Created: 2025-02-16
tags:
  - spring
---
Spring provides several **annotations** to simplify Dependency Injection (DI) and configuration.

#### Dependency Injection

| Annotation   | Description                                                                                                                                                                                        |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `@Autowired` | Used to **automatically inject** a bean (i.e., an object managed by Spring).  <br>It can be applied to **constructors, fields, or setter methods**.                                                |
| `@Qualifier` | Used **with `@Autowired`** when multiple beans of the same type exist.  <br>It helps **specify exactly which bean** should be injected.                                                            |
| `@Primary`   | Used when **multiple beans of the same type exist**, but one should be the **default**.                                                                                                            |
| `@Bean`      | Defines a **Spring-managed bean manually** in a `@Configuration` class.  <br>Useful when **third-party classes** cannot use `@Component`.                                                          |
| `@Lazy`      | Delays bean creation until it is **first requested**, rather than at application startup.<br>**By default, Spring initializes all beans at startup**. `@Lazy` prevents unnecessary initialization. |
| `@Value`     | Injects **values from properties files, system properties, or hardcoded values**.                                                                                                                  |
| `@Scope`     | Defines a bean’s **lifecycle scope** (`singleton`, `prototype`, etc.).                                                                                                                             |
| `@Lookup`    | Used when a **prototype bean** needs to be injected into a **singleton bean**.                                                                                                                     |
### Context Configuration Annotations

| Annotation        | Descrption                                                                                                                               |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `@Profile`        | Activates beans **based on the environment** (e.g., `dev`, `test`, `prod`).  <br>Useful for **conditional bean registration**.           |
| `@Import`         | Imports one `@Configuration` class into another.  <br>**Helps in modularizing configuration**.                                           |
| `@ImportResource` | Loads **XML-based Spring configurations** in a Java-based project.  <br>Useful when **migrating from XML to Java config**.               |
| `@PropertySource` | Loads properties from an external `.properties` file.  <br>Used with `@Value("${property.key}")` to inject values from properties files. |

### Stereotype Annotations
Stereotype annotations are **specialized annotations** in Spring that indicate a class is a **Spring-managed component** (bean). These annotations help Spring automatically detect and register beans in the **Spring IoC container** via **component scanning**.

| Annotation        | Description                                                                                        |
| ----------------- | -------------------------------------------------------------------------------------------------- |
| `@Component`      | Generic annotation that marks a class as a **Spring bean**.                                        |
| `@Service`        | Specialized `@Component` for **business logic** or service layer classes.                          |
| `@Repository`     | Specialized `@Component` for **DAO (Data Access Object) classes**. Handles database-related logic. |
| `@Controller`     | Specialized `@Component` for **Spring MVC controllers**. Handles HTTP requests.                    |
| `@RestController` | Combination of `@Controller` and `@ResponseBody`, used in **RESTful APIs**.                        |
>Spring uses **component scanning** to **automatically detect stereotype annotations**.

Enable Component scanning,
Via Java:
```java
@Configuration
@ComponentScan("com.example")
class AppConfig {}
```

Via XML:
```xml
<context:component-scan base-package="com.example" />
```

