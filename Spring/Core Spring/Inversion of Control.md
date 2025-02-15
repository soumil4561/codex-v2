---
Created: 2025-02-14
tags:
  - spring
  - spring-core
---
**Inversion of Control (IoC)** is a software design principle where the control of object creation and dependency management is transferred from the programmer to a framework or container. Instead of a class creating its dependencies, they are provided externally, making the code more flexible, testable, and loosely coupled.

### Example
Without IOC
```java
public class Service {
    private Repository repository;

    public Service() {
        this.repository = new Repository(); // Dependency is created inside the class
    }
    
    public void doSomething() {
        repository.fetchData();
    }
}
```

With IoC:
```java
public class Service {
    private Repository repository;

    public Service(Repository repository) {
        this.repository = repository;
    }
    
    public void doSomething() {
        repository.fetchData();
    }
}
```

### Types of IOC
1. **Dependency Injection (DI)**: The container injects dependencies into objects.
    - Constructor Injection
    - Setter Injection
    - Field Injection (using `@Autowired` in Spring)
2. **Event-Driven IoC**: The framework reacts to events and manages object lifecycles.

