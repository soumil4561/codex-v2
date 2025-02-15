---
Created: 2025-02-14
tags:
  - spring
  - spring-core
---
Decoupling the traditional dependency b/w objects
Spring-Core module is responsible for injecting dependencies through either Constructor or Setter methods
Suppose class One needs the object of class Two to instantiate or operate a method, then class One is said to be **dependent** on class Two.
[Loose coupling](https://www.geeksforgeeks.org/coupling-in-java/) between classes can be possible by defining [interfaces](https://www.geeksforgeeks.org/interfaces-in-java/) for common functionality and the injector will instantiate the objects of required implementation.

- **Setter Dependency Injection:** Setter DI involves injecting dependencies via setter methods. To configure SDI, the [`@Autowired` annotation](https://www.geeksforgeeks.org/spring-autowired-annotation/) is used along with setter methods, and the property is set through the `<property>` tag in the bean configuration file.

- **Constructor-based Dependency Injection:** Constructor DI involves injecting dependencies through constructors. To configure CDI, the `<constructor-arg>` tag is used in the bean configuration file.

