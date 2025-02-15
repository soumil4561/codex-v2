![[oops_pillars.png]]

## Encapsulation

Encapsulation in C++ is defined as the wrapping up of data and information in a single unit. In Object Oriented Programming, Encapsulation is defined as binding together the data and the functions that manipulate them.

**Two Important  property of Encapsulation** 

1. **Data Protection:** Encapsulation protects the internal state of an object by keeping its data members private. Access to and modification of these data members is restricted to the class’s public methods, ensuring controlled and secure data manipulation.
2. **Information Hiding:** Encapsulation hides the internal implementation details of a class from external code. Only the public interface of the class is accessible, providing abstraction and simplifying the usage of the class while allowing the internal implementation to be modified without impacting external code.

Features:
1. We can not access any function from the class directly. We need an object to access that function that is using the member variables of that class. 
2. The function which we are making inside the class must use only member variables, only then it is called _encapsulation_.
3. If we don’t make a function inside the class which is using the member variable of the class then we don’t call it encapsulation.
4. Encapsulation improves readability, maintainability, and security by grouping data and methods together.
5. It helps to control the modification of our data members.

By **default**, all data members and member functions of a class are made **private** by the compiler.

## Abstraction
Abstraction means displaying only essential information and hiding the details. Data abstraction refers to providing only essential information about the data to the outside world, hiding the background details or implementation.

_**Real-life example of a man driving a car**_. The man only knows that pressing the accelerator will increase the speed of the car or applying brakes will stop the car but he does not know how on pressing the accelerator the speed is actually increasing, he does not know about the inner mechanism of the car or the implementation of the accelerator, brakes, etc in the car. This is what abstraction is.

1. **Data abstraction –** This type only shows the required information about the data and hides the unnecessary data.
2. **Control Abstraction –** This type only shows the required information about the implementation and hides unnecessary information.


**Advantages of Data Abstraction**

- Helps the user to avoid writing the low-level code
- Avoids code duplication and increases reusability.
- Can change the internal implementation of the class independently without affecting the user.
- Helps to increase the security of an application or program as only important details are provided to the user.
- It reduces the complexity as well as the redundancy of the code, therefore increasing the readability.

## Difference b/w them

| Abstraction                                                                                   | Encapsulation                                                                                                                          |
| --------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| Abstraction is the process or method of gaining the information.                              | While encapsulation is the process or method to contain the information.                                                               |
| In abstraction, problems are solved at the design or interface level.                         | While in encapsulation, problems are solved at the implementation level.                                                               |
| Abstraction is the method of hiding the unwanted information.                                 | Whereas encapsulation is a method to hide the data in a single entity or unit along with a method to protect information from outside. |
| We can implement abstraction using abstract class and interfaces.                             | Whereas encapsulation can be implemented using by access modifier i.e. private, protected and public.                                  |
| In abstraction, implementation complexities are hidden using abstract classes and interfaces. | While in encapsulation, the data is hidden using methods of getters and setters.                                                       |
| The objects that help to perform abstraction are encapsulated.                                | Whereas the objects that result in encapsulation need not be abstracted.                                                               |

## Polymorphism
The ability of a message to be displayed in more than one form. **A real-life example of polymorphism** is a person who at the same time can have different characteristics. A man at the same time is a father, a husband, and an employee. So the same person exhibits different behavior in different situations.

### Types of Polymorphism
1. **Compile Time Polymorphism:**
	1. **Function Overloading:** When there are multiple functions with the same name but different parameters, then the functions are said to be **overloaded**. Functions can be overloaded by **changing the number of arguments** or/and **changing the type of arguments**.
	2. **Operator Overloading:** C++ has the ability to provide the operators with a special meaning for a data type, this ability is known as operator overloading. *For example,* we can make use of the addition operator (+) for string class to concatenate two strings. We know that the task of this operator is to add two operands. So a single operator ‘+’, when placed between integer operands, adds them and when placed between string operands, concatenates them. 
2. **Run Time Polymorphism:**
	This type of polymorphism is achieved by **Function Overriding**. *Late binding* and *dynamic polymorphism* are other names for runtime polymorphism. 
	1. **Function Overriding:** Function overriding is a type of polymorphism in which we redefine the member function of a class which it inherited from its base class. The function signature remains same but the working of the function is altered to meet the needs of the derived class. 
	2. **Using Virtual Functions:** A virtual function (also known as virtual methods) is a member function that is declared within a base class and is re-defined (overridden) by a derived class. When you refer to a derived class object using a pointer or a reference to the base class, you can call a virtual function for that object and execute the derived class’s version of the method. [[Virtual Functions]]
	