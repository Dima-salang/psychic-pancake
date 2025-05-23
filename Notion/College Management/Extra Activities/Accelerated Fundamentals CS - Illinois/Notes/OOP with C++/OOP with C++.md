OOP with C++

We can define classes which provides a template for creating entities within the program.

  

We separate the header file and the implementation file from each other. The header file uses an a suffix of “.h” while an implementation file uses the suffix “.cpp”

- The header contains the specification details of the class. It contains the barebones member variables and the functions used within the class
- The implementation is the actual code implementation of the specifications found within the header file.

  

  

We also do have the heap and the stack.

- When we dynamically allocate memory, we allocate the memory into the heap. Actually, we create one for the heap and one for stack. One for the memory address of the dynamic value and the one in the stack for storing the pointer to that dynamic value.

![[Untitled.png]]

![[Untitled 1.png]]

  

  

# Class Constructors

  

### Automatic Default Constructor

- if we do not provide any custom constructors, the C++ compiler provides an automatic default constructor.
- will initialize all member variables to their default values.
- if any custom constructor is defined, an automatic default constructor is not defined.

  

### Custom Default Constructor

- the simplest constructor that can be provided which specifies the state of the object when the object is constructed.
    - A member function with the same name of the class
    - the function takes 0 parameters
    - the functions does not have a return type

  

```JavaScript
\#pragma once

namespace nodes {
    class Node {
        
        public:
            Node();
            int data;
            int next;
    };
}
```

  

  

### Custom Constructor

- we can also set the state of member variables at construction time.

  

```JavaScript
\#pragma once

namespace nodes {
    class Node {
        
        public:
            Node(int data, int next);
            int data;
            int next;
    };
}
```

  

  

# Copy Constructors

- is a special constructor that exists to make a copy of an existing object

  

### Automatic Copy Constructor

- C++ compiler will provide us with one if we do not specify a custom constructors
- it will copy the contents of all member variables.

  

### Custom Copy Constructor

- a class constructor
- has exactly one argument
    - the argument must be const reference of the same type as the class

```C++
\#include "node.h"

namespace nodes {
    Node::Node(int data, int next) {
        this->data = data;
        this->next = next;
    }

    Node::Node(const Node &obj) {
        data = obj.data;
        next = obj.data;
    }
    
}
```

  

  

### Copy Constructor Invocation

- passing an object as a parameter (by value)
- returning an object from a function (by value)
- initializing a new object

  

  

Passing an object as a parameter

```C++
\#include "node.h"
\#include <iostream>
using namespace std;


int returnNode(nodes::Node node) {
    return node.data;
}



int main() {
		// custom constructor
    nodes::Node firstNode(12, 13);
	
		// copies the contents of firstNode to the parameter variable
		// in the stack frame of the returnNode function
    returnNode(firstNode);
    
    system("pause");
    return 0;   
}
```

  

Returning an object from a function

```C++
nodes::Node nodeFactory() {
    nodes::Node secondNode(14, 15);

    return secondNode; 
}



int main() {
    nodes::Node firstNode(12, 13);

    returnNode(firstNode);
		
		// when it gets returned, the instance gets copied to the main function's stack frame
    nodes::Node secondNode = nodeFactory();
    
    system("pause");
    return 0;   
}
```

  

  

Initializing a new object

```C++
int main() {
    nodes::Node firstNode(12, 13);

    returnNode(firstNode);

    nodes::Node secondNode = nodeFactory();

    // copies the contents of thirdNode to fourthNode 
    nodes::Node thirdNode(16, 17);
    nodes::Node fourthNode = thirdNode;
    
    system("pause");
    return 0;   
}
```

  

  

NOTE

- there is a difference between the copy constructor and the assignment operator

  

```C++
int main() {
    nodes::Node firstNode(12, 13);

    returnNode(firstNode);

    nodes::Node secondNode = nodeFactory();

    // copies the contents of thirdNode to fourthNode 
    nodes::Node thirdNode(16, 17);
    nodes::Node fourthNode = thirdNode;


    nodes::Node fifthNode(18, 19);
    nodes::Node sixthNode(20, 21);

    // this will not call the copy constructor.
    // constructor means that we are initializing the object at instantiation
    // but since we already initialized it initially, we are not constructing it
    fifthNode = sixthNode
    
    system("pause");
    return 0;   
}
```

  

  

# Assignment Operator

  

  

if an assignment operator is not provided, the C++ compiler provides an automatic assignment operator.

- will copy the contents of all member variables.

  

  

### Custom Assignment Operator

- a custom assignment operator is:
    
    - is a public member function of the class
    - has the function name operator=
    - has a return value of a reference of the class type
    - has exactly one argument
    - the argument must be const reference of the class type
    
    ```C++
    // assignment operator
                Person &Person::operator=(const Person &obj)
                {
                    fName = obj.fName;
                    lName = obj.lName;
    
                    cout << "assignment operator invoked...\n";
                    return *this;
                }
    ```
    
      
    
      
    
      
    

# Variable Storage

- In C++, an instance of a variable can be stored directly in memory, accessed by pointer, or accessed by reference.

![[Untitled 2.png]]

  

### Direct Storage

- by default, variables are stored directly in memory.
- The type of a variable has no modifiers
- the object takes up exactly its size in memory.

  

### Storage by Pointer

- the type of a variable is modified with an asterisk
- a pointer takes a “memory address width” of memory (ex: 64 bits on a 64-bit system)
- the pointer points to the allocated space of the object.

  

  

### Storage by Reference

- a reference is an alias to existing memory and is denoted in the type with an ampersand
- a reference does not store memory itself, it is only an alias to another variable. it has no size; takes zero bytes to create.
- the alias must be assigned when the variable is initialized.

```C++
Cube &c = cube;
int &i = count;
```

  

  

## Pass By

- identical to storage, arguments can be passed ton functions in three different ways:
    - pass by value (default)
    - pass by pointer (modified with *)
    - pass by reference (modified with &, acts as an alis)

  

### Return By

- similarity, we can also return values by:
    - return by value
    - return by pointer
    - return by reference
        - never return a reference to a stack variable created on the stack of your current function

  

  

# Class Destructors

  

### Automatic Default Destructor

- an automatic default destructor is added to your class if no other destructor is defined
- the only action of the automatic default destructor is to call the default destructor of all member objects
- a destructor should never be called directly. Instead it is automatically called when the object’s memory is being reclaimed by the system:
    - if the object is on the stack, when the function returns
    - if the object is on the heap, when delete is used

  

### Custom Destructor

- to add custom behavior to the end-of-life of the function, a custom destructor can be defined as:
    - a custom destructor is a member function
    - the function’s destructor is the name of the class, preceded by a tilde ~
    - all destructors have zero arguments and no return type

  

```C++
Cube::~Cube() {
	cout << "destroyed" << endl;
}
```

  

  

  

# Segmentation Fault

- if you dereference an address that you shouldn’t, this is often called “segmentation fault,” or “segfault.”
- For example, if you dereference a pointer that is set to nullptr, it will almost certainly cause a segfault and crash immediately.

  

  

  

# Undefined Behavior

- sometimes it’s possible to write a program that compiles, but that is not really a safe or valid program.
- There are some improper ways to write C++ that are not strictly forbidden by the C++ standard, but it does not define how the C++ compiler will handle those situations, so the handling can be different from compiler to compiler. This is called “undefined behavior”.
- “It works for me” is not an excuse. Proofread your code, use safe practices, and avoid relying on undefined behavior.
- Many times, undefined behavior is caused by the careless use of uninitialized variables.

  

# Initialization

- initialization is specifying a value for a variable from the moment it is created.
- remember that if you don’t initialize a pointer, you really should not try to dereference it. if you dereference an uninitialized pointer, even just to read from it and not to write to tit, you may wy cause your program to crash or cause unexpected behavior.

```C++
int *x;
```

- x contains a seemingly random memory address. the program will still compile but this is dangerous code.
- plain built-in types or primitives, such as int, that are not initialized will have unknown values. However, if you have a class type, its default constructor will be used to initialize the object.
- when you want to use heap memory, you’ll use the new operator. as with the stack memory case, you should be wary since these types may not have default initialization. therefore, do not assume primitives will have an expected default value. for those cases, be sure to initialize the value manually.
    
    - you can specify initialization parameters a the end of the new expression. this will create pointer r initialized with newly-allocated memory for an integer with the value 0 explicitly
    
    ```C++
    int *r = new int(0);
    ```
    
- you also need to manually reset pointers to nulltptr when you’re done with them
    - note that using delete on a pointer frees the heap memory allocated at that address. however, deleting the pointer does not change the pointer value itself to nullptr automatically.
- resetting pointers to nullptr will try to solve problems:
    
    - we don’t want to delete the same allocated address more than once by mistake, which could cause errors. Using delete on nullptr does nothing, if we accidentally try to delete the same address twice, nothing further happens.
    - we must never dereference a pointer to deallocated memory. this could cause the program to crash with a segfault, exhibit strange behavior, or cause a security vulnerability.
    
      
    
      
    
      
    
    # Template Types
    
    - is a special type that can take on different types when the type is initialized.
    - std::vector uses a template type
    
      
    
    ![[Untitled 3.png]]
    
      
    

std::vector is a standard library class that provides the functionality of a dynamically growing array with a templated type

Defined in : \#include <vector>

Initialization : std::vector<T> v;

Add to (back) of array: ::push_back(T);

Access specific element: ::operator[](unsigned pos);

Number of elements: ::size()

  

  

When initializing a templated type, the template type goes inside of <> at the end of the type name

```C++
std::vector<char> v1;
std::vector<int> v2;
std::vector<uiuc::Cube> v3;
```

  

# Templated Functions

- a template variable is defined by declaring it before the beginning of a class or function

![[Untitled 4.png]]

  

### Compile-Time Binding

- templated variables are checked at compile time, which allows for errors to be caught before running the program

  

![[Untitled 5.png]]

  

  

# Inheritance

- allows for a class to inherit all member functions and data from a base class into a derived class.

  

### Base Class

- a base class is a generic form of a specialized, derived class
- Shape → Cube