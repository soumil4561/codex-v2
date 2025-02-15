
Object-oriented programming (OOP) is a computer programming model that organizes software design around data, or [objects](https://www.techtarget.com/searchapparchitecture/definition/object), rather than functions and logic. 
OOP focuses on the objects that developers want to manipulate rather than the logic required to manipulate them. 
This approach to programming is well suited for software that is large, complex and actively updated or maintained.

Object-oriented programming has several advantages over procedural programming:

- OOP is faster and easier to execute
- OOP provides a clear structure for the programs
- OOP helps to keep the C++ code DRY "Don't Repeat Yourself", and makes the code easier to maintain, modify and debug
- OOP makes it possible to create full reusable applications with less code and shorter development time

**Classes:** A class is a user-defined data type that we can use in our program, and it works as an object constructor, or a "blueprint" for creating objects.
Attributes and methods are basically **variables** and **functions** that belongs to the class. These are often referred to as "class members".

**Objects:** An object is nothing but a self-contained component that consists of methods and properties to make a data useful. It helps you to determines the behavior of the class.

| Class                                                             | Object                                                   |
| ----------------------------------------------------------------- | -------------------------------------------------------- |
| A class is a template for creating objects in program.            | The object is an instance of a class.                    |
| A class is a logical entity                                       | Object is a physical entity                              |
| A class does not allocate memory space when it is created.        | Object allocates memory space whenever they are created. |
| You can declare class only once.                                  | You can create more than one object using a class.       |
| Classes can’t be manipulated as they are not available in memory. | They can be manipulated.                                 |

### Access Modifiers
We can control the access to the members of the class using Access Specifiers. Also known as [access modifier](https://www.geeksforgeeks.org/access-modifiers-in-c/), they are the keywords that are specified in the class and all the members of the class under that access specifier will have particular access level.
#### In C++
1. **Public:** Members declared as public can be accessed from outside the class.
2. **Private** Members declared as private can only be accessed within the class itself.
3. **Protected:** Members declared as protected can be accessed within the class and by derived classes.
#### In Java
1. **Private**: The access level of a private modifier is only within the class. It cannot be accessed from outside the class.
2. **Default**: The access level of a default modifier is only within the package. It cannot be accessed from outside the package. If you do not specify any access level, it will be the default.
3. **Protected**: The access level of a protected modifier is within the package and outside the package through child class. If you do not make the child class, it cannot be accessed from outside the package.
4. **Public**: The access level of a public modifier is everywhere. It can be accessed from within the class, outside the class, within the package and outside the package.