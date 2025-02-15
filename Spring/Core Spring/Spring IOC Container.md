---
Created: 2025-02-14
tags:
  - spring
  - spring-core
---

The **Spring IoC Container** is the core of the Spring Framework that manages the lifecycle and dependencies of Spring beans. It implements **Inversion of Control (IoC)**, meaning it takes control of object creation and dependency injection, instead of the developer manually instantiating objects.

### Types
#### 1. **BeanFactory (Basic IoC Container)**

- It is the simplest container and is lightweight.
- Uses **lazy loading** (creates beans only when needed).
- Implemented by `org.springframework.beans.factory.BeanFactory`.
- Example implementation: `XmlBeanFactory` (deprecated, now replaced by `ClassPathXmlApplicationContext`).
#### 2. **ApplicationContext (Advanced IoC Container)**

- Provides all features of `BeanFactory` plus additional functionalities.
- Supports **eager loading** (creates beans at startup).
- Provides event propagation, declarative mechanisms (like AOP), and internationalization support.
- Implemented by `org.springframework.context.ApplicationContext`.

### Key Features of Spring IoC Container

1. **Bean Management**: The container manages the lifecycle and dependencies of objects (Spring beans).
2. **Dependency Injection (DI)**: Automatically injects required dependencies.
3. **Configuration Support**: Beans can be defined using **XML**, **Java Annotations**, or **Java Config classes**.
4. **Event Handling**: Supports application event propagation.
5. **Internationalization (i18n)**: Helps in message localization.

![[Screenshot 2025-02-14 at 12.02.06 PM.png]]

The object ask Spring for the instance from the Bean Factory. In turn, the bean factory reads the configuration for that object in the config file, instantiates the object and returns the object back to the caller. This way the bean factory has control over the object and its lifecycle.

### Using XML
```xml
<!-- beans.xml -->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <bean id="myService" class="com.example.MyService"/>
</beans>
```
### Using AppConfig
```java
@Configuration
public class AppConfig {
    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```

```java
// Java Code to Load XML Configuration
ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
MyService service = context.getBean("myService", MyService.class);
service.performAction();
```


### Spring Bean Scopes
Spring provides **bean scopes** to define how and when a bean instance is created and shared within the application.

1. **Singleton Scope** 
	- Only one instance of the bean is created per Spring container.
	- This instance is **shared** across the application.
	- Initialized at startup (unless lazy initialization is used).
```xml
	<bean id="myService" class="com.example.MyService" scope="singleton"/>
```

```java
@Bean
@Scope("singleton")
public MyService myService() {
    return new MyService();
}
```

2. **Property Scope** 
	- A new instance is created every time the bean is requested.
	- Suitable for **stateful beans** where each request should have a fresh instance.
	- **Not managed by Spring after creation** (no automatic destruction).
3. **Request Scope**
	- A **new instance is created per HTTP request** in a Spring web application.
	- Used when beans should be **request-scoped**, like form data handling.
```java
@Bean
@Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
public MyService myService() {
    return new MyService();
}
```
4. **Session Scope**
	- A **new instance is created per HTTP session** in a Spring web application.
	- Useful for storing **user session data**.
```java
@Bean
@Scope(value = WebApplicationContext.SCOPE_SESSION, proxyMode = ScopedProxyMode.TARGET_CLASS)
public MyService myService() {
    return new MyService();
}
```
5. **Application Scope**
	- A **single instance per ServletContext** (shared across the whole application).
	- Useful for **global configuration settings**.
```java
@Bean
@Scope(value = WebApplicationContext.SCOPE_APPLICATION)
public MyService myService() {
    return new MyService();
}
```
6. **WebSocket Scope**
	- A **new instance is created per WebSocket session**.
	- Useful for **real-time messaging applications**.
```java
@Bean
@Scope(value = "websocket", proxyMode = ScopedProxyMode.TARGET_CLASS)
public MyService myService() {
    return new MyService();
}
```

| Scope                   | Instance Creation                  | Suitable For                          |
| ----------------------- | ---------------------------------- | ------------------------------------- |
| **singleton** (default) | One instance per container         | Stateless beans, shared resources     |
| **prototype**           | New instance on every request      | Stateful beans, non-shared components |
| **request**             | One instance per HTTP request      | Web apps, form data handling          |
| **session**             | One instance per HTTP session      | Web apps, user sessions               |
| **application**         | One instance per ServletContext    | Web apps, global configurations       |
| **websocket**           | One instance per WebSocket session | WebSocket-based applications          |
