---
Created: 2025-02-14
tags:
  - spring
  - spring-core
---

Spring provides several lifecycle callbacks that allow us to perform actions **before a bean is ready for use** and **before it gets destroyed**.

### Lifecycle Phases
A Spring bean goes through these phases:

1. **Instantiation** → Object is created.
2. **Populating Properties** → Dependencies are injected.
3. **Initialization** → Custom logic is executed before the bean is ready.
4. **Ready for Use** → The bean is now available.
5. **Destruction** → Cleanup is performed before the bean is removed.

### Lifecycle Callback Methods
1. **Implementing `InitializingBean` and `DisposableBean` Interfaces**
```java
public class MyBean implements InitializingBean, DisposableBean {

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("Bean is initializing...");
    }

    @Override
    public void destroy() throws Exception {
        System.out.println("Bean is being destroyed...");
    }
}
```
**Not recommended** as it couples the class with Spring. Prefer annotations or XML.

2. **Using `init-method` and `destroy-method` in XML**
```java
public class MyBean {
    
    @PostConstruct
    public void init() {
        System.out.println("Bean is initializing...");
    }

    @PreDestroy
    public void cleanup() {
        System.out.println("Bean is being destroyed...");
    }
}
```

3. **Using `@PostConstruct` and `@PreDestroy` Annotations**
```java
import jakarta.annotation.PostConstruct;
import jakarta.annotation.PreDestroy;

public class MyBean {
    
    @PostConstruct
    public void init() {
        System.out.println("Bean is initializing...");
    }

    @PreDestroy
    public void cleanup() {
        System.out.println("Bean is being destroyed...");
    }
}
```

4.  **Using a `BeanPostProcessor`**
```java
@Component
public class MyBeanPostProcessor implements BeanPostProcessor {

    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) {
        System.out.println("Before Init: " + beanName);
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) {
        System.out.println("After Init: " + beanName);
        return bean;
    }
}
```
If you need **global processing** on all beans, use `BeanPostProcessor`.


