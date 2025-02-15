---
Created: 2025-02-14
tags:
  - spring
  - spring-core
---

#### Basic
```xml
<bean id="myService" class="com.example.MyService"/>
```

#### Setter Injection
- Injects dependencies using setter methods.
```xml
<bean id="myService" class="com.example.MyService">
    <property name="repository" ref="repository"/>
</bean>
```

#### Constructor Injection
```xml
<bean id="myService" class="com.example.MyService"> 
	<constructor-arg name="x" value="20" />
</bean>
```

`type` tag can be used for setting the type
`index` can be used for numbering 
`ref` can be used for referencing to another bean (another object)
`autowire` set the autowire property to `byName` `byType` `constructor`

#### Injecting collections
```xml
<bean id="person" class="com.example.Person">
    <property name="hobbies">
        <list>
            <value>Reading</value>
            <value>Coding</value>
            <value>Gaming</value>
        </list>
    </property>
</bean>
```
for set:
```xml
<bean id="person" class="com.example.Person">
    <property name="skills">
        <set>
            <value>Java</value>
            <value>Spring</value>
        </set>
    </property>
</bean>
```
for map:
```xml
<bean id="person" class="com.example.Person">
    <property name="contacts">
        <map>
            <entry key="home" value="1234567890"/>
            <entry key="work" value="0987654321"/>
        </map>
    </property>
</bean>
```


#### Inner Beans
```xml
<bean id="myService" class="com.example.MyService">
    <property name="repository">
        <bean class="com.example.Repository"/>
    </property>
</bean>
```

#### Load App.properties values here
```xml
<context:property-placeholder location="classpath:app.properties"/>

<bean id="databaseConfig" class="com.example.DatabaseConfig">
    <property name="url" value="${db.url}"/>
    <property name="username" value="${db.username}"/>
    <property name="password" value="${db.password}"/>
</bean>
```


