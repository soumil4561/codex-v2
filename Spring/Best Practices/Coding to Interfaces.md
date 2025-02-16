**"Coding to interfaces"** means writing code that depends on **interfaces** rather than **concrete implementations**. This approach improves **flexibility, maintainability, and testability**.

#### Example
```java
// Define an interface
interface Engine {
    void start();
}

// Implementations of Engine
class PetrolEngine implements Engine {
    public void start() {
        System.out.println("Petrol engine starting...");
    }
}

class DieselEngine implements Engine {
    public void start() {
        System.out.println("Diesel engine starting...");
    }
}

// Car depends on the Engine interface, not a concrete class
class Car {
    private Engine engine;

    // Constructor injection (Dependency Injection)
    public Car(Engine engine) {
        this.engine = engine;
    }

    void startCar() {
        engine.start();
    }
}
```

*Furthermore think like this in the same example: *

Suppose we have multiple implementations of the `Engine` interface, and we define them as beans in the Spring container. Instead of retrieving a specific implementation like this:

```java
PetrolEngine petrolEngine = (PetrolEngine) context.getBean("petrolEngine");
petrolEngine.start();
```

We can simply write:
```java
Engine engine = (Engine) context.getBean("petrolEngine");
engine.start();
```
 As long as petrol engine implements the Engine interface.

### **Advantages of Using Interfaces When Retrieving Beans**

1. **Loose Coupling**
    
    - Your code depends on the `Engine` interface, **not on the `PetrolEngine` class**.
    - Later, if you want to switch to `DieselEngine`, you only need to change the Spring bean configuration, **not the Java code**.
2. **Easier to Switch Implementations**
    
    - If `PetrolEngine` is replaced by `DieselEngine`, we don’t need to modify `Car.java` or other dependent classes.
    - Just update the Spring configuration to return a `DieselEngine` when `context.getBean("dieselEngine")` is called.

#### Using with Spring Boot

##### 1. Using the @Primary annotation
```java
@Component
@Primary  // This bean is injected by default when no qualifier is specified
class PetrolEngine implements Engine {
    public void start() {
        System.out.println("Petrol engine starting...");
    }
}

@Component
class DieselEngine implements Engine {
    public void start() {
        System.out.println("Diesel engine starting...");
    }
}

@Component
class Car {
    private final Engine engine;

    @Autowired
    public Car(Engine engine) {  // Will inject PetrolEngine by default
        this.engine = engine;
    }

    void startCar() {
        engine.start();
    }
}
```

##### 2. Using the @Qualifier annotation
```java
@Component
class PetrolEngine implements Engine {
    public void start() {
        System.out.println("Petrol engine starting...");
    }
}

@Component
class DieselEngine implements Engine {
    public void start() {
        System.out.println("Diesel engine starting...");
    }
}

@Component
class Car {
    private final Engine engine;

    @Autowired
    public Car(@Qualifier("dieselEngine") Engine engine) { // Explicitly specify
        this.engine = engine;
    }

    void startCar() {
        engine.start();
    }
}
```