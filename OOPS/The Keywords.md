
## Static 

### Static Variables
**Static variables in a Function**: When a variable is declared as static, space for **it gets allocated for the lifetime of the program**. Even if the function is called multiple times, space for the static variable is allocated only once and the value of the variable in the previous call gets carried through the next function call.

**Static variables in a class**: As the variables declared as static are initialized only once as they are allocated space in separate static storage so, the static variables **in a class are shared by the objects.** There can not be multiple copies of the same static variables for different objects. 

Also because of this reason static variables can not be initialized using constructors.

### Static Members of a class
**Class objects as static**: Just like variables, objects also when declared as static have a scope till the lifetime of the program. Consider the below program where the object is non-static.

**Static functions in a class**: static member functions also do not depend on the object of the class. 

*We are allowed to invoke a static member function using the object and the ‘.’ operator but it is recommended to invoke the static members using the class name and the scope resolution operator.*

**Static member functions are allowed to access only the static data members or other static member functions**

## This 
Each created object gets its own copy of data members and all objects share a single copy of member functions. 
*if only one copy of each member function exists and is used by multiple objects, how are the proper data members are accessed and updated?*
The compiler supplies an implicit pointer along with the names of the functions as ‘this’.

The ‘this’ pointer is passed as a hidden argument to all nonstatic member function calls and is available as a local variable within the body of all nonstatic functions. **Not for static member functions.**

For a class X, the type of this pointer is ‘X* ‘. Also, if a member function of X is declared as const, then the type of this pointer is ‘const X *’

## New
The [new operator](https://www.geeksforgeeks.org/new-and-delete-operators-in-cpp-for-dynamic-memory/) is an **operator** which denotes a request for memory allocation on the Heap. If sufficient memory is available, new operator initializes the memory and returns the address of the newly allocated and initialized memory to the [pointer](https://www.geeksforgeeks.org/cpp-pointers/) variable.

When new is used to create a new object:
- The memory for the object is allocated using **operator new** from heap.
- The constructor of the class is invoked to properly initialize this memory.
### Operator new
Operator new is a **function** that allocates raw memory and conceptually a bit similar to [malloc()](https://www.geeksforgeeks.org/cpp-malloc/).
- It is the mechanism of overriding the default [heap](https://www.geeksforgeeks.org/heap-data-structure/) allocation logic.
- It doesn’t initializes the memory i.e constructor is not called. However, after our overloaded new returns, the compiler then automatically calls the constructor also as applicable.
- It’s also possible to [overload operator new](https://www.geeksforgeeks.org/overloading-new-delete-operator-c/) either globally, or for a specific class

1. **Operator vs function:** new is an operator as well as a keyword whereas operator new is only a function.
2. **New calls “Operator new”:** “new operator” calls “operator new()” , like the way + operator calls operator +()
3. **“Operator new” can be Overloaded:** Operator new can be overloaded just like functions allowing us to do customized tasks.
4. **Memory allocation:** ‘new expression’ call ‘operator new’ to allocate raw memory, then call constructor.
## Const

### Const Variables
Whenever **const keyword** is attached with any method(), variable, [pointer variable](https://www.geeksforgeeks.org/pointers-in-c-and-c-set-1-introduction-arithmetic-and-array), and with the object of a class it prevents that specific **object/method()/variable** to modify its data items value.

Rules for declaring a const

![[t1.png]]


### Const with Pointers
**When the pointer variable point to a const value**
```cpp
const data_type* var_name;
```

**When the const pointer variable point to the value:**
```cpp
data_type* const var_name;
```

**When const pointer pointing to a const variable:**
```cpp
const data_type* const var_name;
```

**Further Reading Required**

### Const Functions



## Final
In Java, we can use [final](http://geeksquiz.com/java/final-keyword/) for a function to make sure that it cannot be overridden. We can also use final in Java to make sure that a class cannot be inherited. 
Sometimes you don’t want to allow derived class to override the base class’ virtual function. [C++ 11](http://en.wikipedia.org/wiki/C%2B%2B11) allows built-in facility to prevent overriding of virtual function using final specifier.

can also be used to prevent inheritance of class / struct. If a class or struct is marked as final then it becomes non inheritable and it cannot be used as base class/struct.

*Note that use of final specifier in C++ 11 is same as in Java but Java uses final before the class name while final specifier is used after the class name in C++ 11. Same way Java uses final keyword in the beginning of method definition (Before the return type of method) but C++ 11 uses final specifier after the function name.*

## Virtual
