---
Created: 2025-02-16
tags:
  - spring
  - java
---
### What are annotations?
One word to explain _annotation is metadata_. Metadata is data about data. So annotations are metadata for code. For example, look at the following piece of code.
```java
@Override
public String toString() {
return "This is String Representation of current object.";
}
```

Used @Override to override the `toString` method. Even if we don’t put `@Override`, code works properly without any issue. `@Override` tells the compiler that this method is an overridden method (metadata about the method), and if any such method does not exist in a parent class, then throw a compiler error (method does not override a method from its super class).

*Annotation is a special kind of Java construct used to decorate a class, method, field, parameter, variable, constructor, or package. It’s the vehicle chosen by JSR-175 to provide metadata.*

### Why were they introduced?
Before annotations, **Java relied heavily on XML and marker interfaces** for configuration, which had problems like:  
❌ **Verbose and hard-to-maintain XML configurations** (e.g., in EJB, Struts, Spring XML).  
❌ **Marker interfaces** (like `Serializable`) were limited and couldn't store additional metadata.  
❌ **Reflection-based configurations** were complex and slow.

**Annotations were introduced in Java 5 (2004) to:**  
✅ Replace XML configurations.  
✅ Provide **metadata at compile-time or runtime** without affecting logic.  
✅ Make **frameworks like Spring, Hibernate, and JUnit more powerful and concise**

They were introduced because devs wanted something that could be coupled closely with code instead of XML, which is very loosely coupled (in some cases, almost separate) from code.

### How do they work?

>**Annotations are only metadata and do not contain any business logic**.

They are processed at **compile-time** or **runtime**, depending on their **retention policy**.

Annotations can be:  
1️⃣ **Checked by the compiler** (e.g., `@Override`)  
2️⃣ **Used by tools** (e.g., documentation via `@Documented`)  
3️⃣ **Processed at runtime** using **reflection** (e.g., `@Autowired` in Spring)

#### Compilation-Time Annotations
Some annotations **help the compiler enforce rules** and avoid errors.

```java
class Parent {
    void display() {}
}

class Child extends Parent {
    @Override
    void display() {  // ✅ Correct override, compiler is happy!
        System.out.println("Overridden method");
    }

    // @Override  // ❌ ERROR: No such method in Parent class!
    // void show() {}
}
```

#### Run-Time Annotations
Some annotations are **retained at runtime** and can be accessed via **reflection**.

```java
import java.lang.annotation.*;
import java.lang.reflect.*;

// Define an annotation with RUNTIME retention
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@interface MyAnnotation {
    String value();
}

// Apply annotation to a method
class Test {
    @MyAnnotation("Hello, Reflection!")
    public void myMethod() {}
}

// Use reflection to process the annotation
public class AnnotationProcessor {
    public static void main(String[] args) throws Exception {
        Method method = Test.class.getMethod("myMethod");

        if (method.isAnnotationPresent(MyAnnotation.class)) {
            MyAnnotation annotation = method.getAnnotation(MyAnnotation.class);
            System.out.println("Annotation Value: " + annotation.value());
        }
    }
}
```

#### Annotation Processing at Compile Time
Some annotations, like **Lombok (`@Getter`, `@Setter`)** and **JPA (`@Entity`)**, are processed during compilation to **generate additional code**.

### **Step-by-Step Execution of an Annotation:**

1️⃣ **Compiler reads annotation metadata**  
2️⃣ **Depending on retention policy**:
- `SOURCE`: Removed after compilation
- `CLASS`: Kept in class files but not available at runtime
- `RUNTIME`: Available at runtime via reflection  
3️⃣ **Frameworks (like Spring) process annotations** using reflection or AOP (Aspect-Oriented Programming).  
4️⃣ **Annotations may modify behavior**, generate code, or inject dependencies.

### **What Can Annotations Do?**

1️⃣ **Provide Information to the Compiler** 🏗️  
2️⃣ **Generate Code at Compile-Time** 📝  
3️⃣ **Modify Class Behavior at Runtime** ⚙️  
4️⃣ **Inject Dependencies (DI in Spring)** 🔄  
5️⃣ **Map Java Classes to Other Representations (ORM, Serialization)** 📊  
6️⃣ **Enable Aspect-Oriented Programming (AOP)** 🎯

### Make your own Annotations
Annotations are defined using `@interface`.

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME) // Available at runtime
@Target(ElementType.METHOD)         // Can be applied to methods
@interface MyAnnotation {
    String value(); // Annotation attribute

	//To create one with some default value
	String name() default "some name";
}
```

This annotation has a **single attribute** (`value()`).  
**Retention policy** controls when the annotation is available.

```java
class Test {
    @MyAnnotation("Hello, Annotation!")  // Using the annotation
    public void myMethod() {}
}
```
Use the annotation on your method

```java
import java.lang.reflect.*;

public class AnnotationProcessor {
    public static void main(String[] args) throws Exception {
        Method method = Test.class.getMethod("myMethod");

        if (method.isAnnotationPresent(MyAnnotation.class)) {
            MyAnnotation annotation = method.getAnnotation(MyAnnotation.class);
            System.out.println("Annotation Value: " + annotation.value());
        }
    }
}
```
**Read annotation values at runtime**

### Meta-Annotations
Java provides **meta-annotations** to control annotation behavior:

| Meta-Annotation | Description                                                                 |
| --------------- | --------------------------------------------------------------------------- |
| `@Retention`    | Defines how long the annotation is available (`SOURCE`, `CLASS`, `RUNTIME`) |
| `@Target`       | Defines where the annotation can be used (`FIELD`, `METHOD`, `TYPE`, etc.)  |
| `@Documented`   | Includes the annotation in JavaDocs                                         |
| `@Inherited`    | Allows annotation to be inherited by subclasses                             |
| `@Repeatable`   | Allows multiple instances of the same annotation                            |
```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD, ElementType.TYPE}) // Can be used on classes & methods
@Documented
@interface Info {
    String author();
    String date();
}
```

```java
@Info(author = "John", date = "2025-02-16")
class MyClass {}
```

#### Making Repeatable Annotations
Since Java 8, you can use the same Annotations multiple times. 

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Repeatable(Authors.class)
@interface Author {
    String name();
}

// Container annotation
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@interface Authors {
    Author[] value();
}
```
`@Repeatable(Authors.class)`: This tells Java that multiple `@Author` annotations should be **grouped inside a container annotation** (`@Authors`).

What is `@Authors`
- This **acts as a wrapper** for multiple `@Author` annotations.
- It has an array of `@Author` as its value (`Author[] value();`).

How to use it:
```java
@Author(name = "Alice")
@Author(name = "Bob")
class Book {}
```

Access it:
```java
public class Main {
    public static void main(String[] args) {
        // Get all @Author annotations applied to the Book class
        Author[] authors = Book.class.getAnnotationsByType(Author.class);

        for (Author author : authors) {
            System.out.println("Author: " + author.name());
        }
    }
}
```

Java **automatically groups** multiple `@Author` annotations into a **single `@Authors` annotation**.


### Some useful Annotations in Java
| Annotation             | Purpose                                                    |
| ---------------------- | ---------------------------------------------------------- |
| `@Override`            | Ensures a method overrides a superclass method.            |
| `@Deprecated`          | Marks a method/class as outdated.                          |
| `@SuppressWarnings`    | Suppresses compiler warnings.                              |
| `@FunctionalInterface` | Ensures an interface has **only one** abstract method.     |
| `@SafeVarargs`         | Suppresses unsafe operations on generic varargs.           |
| `@Native`              | Indicates a constant is meant for native code interaction. |

