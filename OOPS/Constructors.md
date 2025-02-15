A special method that is invoked automatically at the time an object of a class is created. It is used to initialize the data members of new objects generally. The constructor in C++ has the same name as the class or structure.

**Syntax in C++**
```cpp
<class-name> (){  
...  
}
```

- The name of the constructor is the same as its class name.
- Constructors are mostly declared in the public section of the class though they can be declared in the private section of the class.
- Constructors do not return values; hence they do not have a return type.
- A constructor gets called automatically when we create the object of the class.

## Types 
1. **Default Constructor:** A default constructor is a constructor that doesn’t take any argument. It has no parameters. The compiler automatically creates an implicit default constructor if the programmer does not define one.
2. **Parametrized Constructor:** Parameterized constructors make it possible to pass arguments to constructors. Typically, these arguments help initialize an object when it is created.
3. **Copy Constructor:** A copy constructor is a member function that initializes an object using another object of the same class. Copy constructor takes a reference to an object of the same class as an argument.
```cpp
ClassName (ClassName &obj){  
  __// body_containing_logic__  
}
```

Just like the default constructor, the C++ compiler also provides an implicit copy constructor if the explicit copy constructor definition is not present.

4. **Move Constructor:** The move constructor is a recent addition to the family of constructors in C++. It is like a copy constructor that constructs the object from the already existing objects., but instead of copying the object in the new memory, it makes use of move semantics to transfer the ownership of the already created object to the new object without creating extra copies.
	- The move constructor takes the [rvalue reference](https://www.geeksforgeeks.org/lvalues-references-and-rvalues-references-in-c-with-examples/) of the object of the same class and transfers the ownership of this object to the newly created object.
	- Like a copy constructor, the compiler will create a move constructor for each class that does not have any explicit move constructor.
	```cpp
	className (className&& obj) {  
     __// body of the constructor__  
	}
```


## Destructors
Destructor is an instance member function that is invoked automatically whenever an object is going to be destroyed. 


```cpp
<class-name> {  
public:  
     ~<class-name>(); 
}  
  
<class-name> :: ~<class-name>() {
   _ // some instructions****_  
}
```

*The upper one for defining inside class, while the below one for outside class. we still need to at least declare the destructor inside the class.*

- A destructor is also a special member function like a constructor. Destructor destroys the class objects created by the constructor. 
- Destructor has the same name as their class name preceded by a tilde (~) symbol.
- It is not possible to define more than one destructor.
- The destructor is only one way to destroy the object created by the constructor. Hence, destructor cannot be overloaded.
- It cannot be declared static or const.
- Destructor neither requires any argument nor returns any value.
- It is automatically called when an object goes out of scope. 
- Destructor release memory space occupied by the objects created by the constructor.
- In destructor, objects are destroyed in the reverse of an object creation.

#### When is it called?
1. Destructor is called when the function ends.
2. Destructor is called when the program ends.
3. Destructor is called when a block containing local variables ends.
4. Destructor is called when a delete operator is called.

*Call it as you would call any normal method of class. It has the same name as the class*
```cpp
object_name.~class_name()
```

#### Private Destructors
Destructors with the [access modifier](https://www.geeksforgeeks.org/access-modifiers-in-c/) as private are known as Private Destructors. Whenever we want to prevent the destruction of an object, we can make the destructor private.

Whenever we want to control the destruction of objects of a class, we make the destructor private. For dynamically created objects, it may happen that you pass a pointer to the object to a function and the function deletes the object.

**Further Reading Required for Private Destructors. Come back after dynamic memory allocation and pointer theory**

