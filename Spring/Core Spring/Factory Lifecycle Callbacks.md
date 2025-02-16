---
Created: 2025-02-16
tags:
  - spring
  - spring-core
---

### Spring `BeanFactoryPostProcessor`

`BeanFactoryPostProcessor` is an **interface** that allows modifying the **bean definitions** before the Spring container actually instantiates the beans.

is different from `BeanPostProcessor` because it **operates on the BeanFactory level (bean definitions)**, not on individual bean instances.

#### Example
```java
@Component
public class MyBeanFactoryPostProcessor implements BeanFactoryPostProcessor {

    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) {
        System.out.println("Inside BeanFactoryPostProcessor");

        BeanDefinition beanDefinition = beanFactory.getBeanDefinition("myBean");
        beanDefinition.getPropertyValues().add("message", "Modified by BeanFactoryPostProcessor");
    }
    
	//Targeting a Specific Bean
	if (beanFactory.containsBeanDefinition("specialBean")) { 
		BeanDefinition beanDefinition = beanFactory.getBeanDefinition("specialBean"); 
		beanDefinition.setScope("prototype"); // Change scope dynamically
	}
}
```

*Make sure to define the same in your spring.xml as:*
```xml
<bean class="org.example.MyBeanFactoryPostProcessor"/>
```

### Using the `PropertyPlaceholderConfigurer`

`PropertyPlaceholderConfigurer` is a special **BeanFactoryPostProcessor** that **replaces placeholders** in bean definitions with values from **external properties files**.

- **Separates configuration from code** (externalized properties).
- **Allows changing values** without modifying the application.
- **Replaces placeholders (`${}`) in XML and annotations.**

#### Example
*Create the app.properties*
```properties
db.url=jdbc:mysql://localhost:3306/mydb
db.username=root
db.password=secret
```

*Add to xml config*
```xml
<bean id="propertyConfigurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
    <property name="location" value="classpath:app.properties"/>
</bean>   %%Define the Property Placeholder Configurer %%

<bean id="databaseConfig" class="com.example.DatabaseConfig">
    <property name="url" value="${db.url}"/>
    <property name="username" value="${db.username}"/>
    <property name="password" value="${db.password}"/>
</bean> 
```

***Now the said values will be instantiated from the Properties file***

| **Feature**             | **PropertyPlaceholderConfigurer**       | **BeanFactoryPostProcessor**                   |
| ----------------------- | --------------------------------------- | ---------------------------------------------- |
| Purpose                 | Injects values from `.properties` files | Modifies bean definitions before instantiation |
| When It Runs            | Before bean creation                    | Before bean instantiation                      |
| Works On                | Placeholder values (`${}`)              | Bean definitions                               |
| Spring Boot Alternative | `PropertySourcesPlaceholderConfigurer`  | BeanFactoryPostProcessor                       |
