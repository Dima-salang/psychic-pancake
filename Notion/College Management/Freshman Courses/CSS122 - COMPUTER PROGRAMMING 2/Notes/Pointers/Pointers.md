  

> [!important] A pointer is a variable which contains the address in memory of another variable. We can have a pointer to any variable type.

A pointer variable is a variable whose content is an address, that is, a memory location and the pointer variable is said to point to that memory location.

- It is a construct that gives you more control of the computer’s memory.
- A computer’s memory is divided into numbered memory locations
- And that variables are implemented as a sequence of adjacent memory locations.

  

The unary or monadic operator & gives the “address of a variable”.

The indirection or dereferencing operator * gives the “contents of an object” pointed to by a pointer.

  

C/C++ uses pointers a lot.

- It is the only way to express some computations
- It produces compact and efficient code
- It provides a very powerful tool.

  

The value of a pointer variable is an address or memory space. Therefore, when you declare a pointer variable, you also specify the data type of the value to be stored in the memory location pointed to by the pointer variable.

```C++
dataType *identifier;

int *p;
char *ch;
```

- The content of p points to a memory location of type int. So, p is a pointer variable of type int.

  

Note:

```C++
int *p;
// is equivalent to
int* p;
// is equivalent to
int * p;
```

- Thus, the character * can appear anywhere between the data type name and the variable name.

  

```C++
int* p, q;
```

- In this statement, only p is the pointer variable, not q. q is an int variable.
- Each variable must have its own * character placed to the left of it to make it a pointer variable.

  

  

### Address of Operator

- The ampersand, &, called the address of operator, is a unary operator that returns the address of its operand.

```C++
int x;
int *p;

p = &x;
```

- x and the value of p refer to the same memory location.

  

```C++
int num1, num2;
int *numPtr;

num1 = 100;
num2 = num1;

numPtr = &num1;

cout << num1 << endl;
cout << numPtr << endl;
```

- The name num1 now stands in place of an int memory location and can be assigned the value of any integer between a certain range.
- The value held at the memory location named num1 can easily be copied directly to another integer memory location.
- The name num1 in a sense means “the value held in the memory location named num1”. This value is copied into the memory location named num2.
- When we use numPtr = &num1;
    - we are asking the program to copy the address of the memory location of num1 to numPtr, not the integer value it is holding. numPtr will not contain 100, but the actual address num1 has been assigned to by the OS. This makes numPtr a reference to num1.

  

### Dereferencing Operator

- When used as a unary operator, * refers to the object to which its operand points.

  

  

### Initializing Pointer Variables

- Because C++ does not automatically initialize variables, pointer variables must be initialized if you do not want them to point to anything.
- Pointer variables are initialized using the constant value 0, called the null pointer.
- Some programmers use NULL. The named constant NULL is defined in the header file cstddef.

```C++
p = NULL;
p = 0;
```

- THey are both equal
- The number 0 is the only number that can be directly assigned to a pointer variable.

  

Using nullptr

- In C++11 Standard, nullptr can be used to initialize pointer variables.
- A pointer with the value nullptr points to nothing, and is called the null pointer. nullptr has a special value type that can be converted to any pointer type.

  

```C++
int *p = nullptr;
```

  

## Basic Memory Management

### Freestore or Heap

- A special area of memory
- Reserved for dynamically allocated variables
- Any new dynamic variable created by a program consumes some of the memory in the freestore
- If your program creates too many dynamic variables, it will consume all the memory in the freestore. If this happens, any additional calls to new will fail.
- C++ compilers, if there was insufficient available memory to create a new variable, then new returns a special value named NULL.
- If there is insufficient available memory to create the new variable, the new operator terminates the program.

> [!important]
> 
> **Freestore** - an abstract concept in C++ for dynamically allocated memory

  

> [!important]
> 
> **Heap** - a data structure that can be used to manage dynamic memory and is used by compilers to implement the freestore.

### Memory Leaks

- occurs when programmers allocate memory using new but forget to deallocate it with delete or delete[].
- This results in memory that remains allocated but is no longer used by the program, leading to increased memory usage over time.
- Over time, the available memory may diminish, potentially slowing down the system or causing the program to crash due to lack of memory.

  

```C++
    // infinite loop to test crash
    while (true)
    {   
        // allocate memory for an array with trillion values
        // will consume a lot of memory and will eventually result to std::bad_alloc
        // until the program consumes the main memory
        num = new int[1,000,000,000,000];
    }
```

### Dynamic Variables

- we learned how to store the address of a variable into a pointer variable of the same type as the variable, and how to manipulate data using pointers. However, you learned how to use pointers to manipulate data only into memory spaces that were created using other variables. In other words, the pointers manipulated data into already existing memory spaces.
- However, we can also allocate and deallocate memory during program execution using pointers. This is the power of pointers.

  

Variables that are created during program execution are called **dynamic variables.** With the help of pointers, C++ creates dynamic variables. C++ provides two operators, new and delete, to create and destroy dynamic variables.

  

### Operator new

- The operator new has two forms: one to allocate a single variable and another to allocate an array of variables

```C++
new dataType;
new dataType[intExp];
```

- The operator new allocates memory (as a variable) of the designated type and returns a pointer to it— that is, the address of this allocated memory. Moreover, the allocated memory is uninitialized.
- When we allocate memory using new, we are allocating it on the freestore.

  

### Operator delete

- The delete operator eliminates a dynamic variable and returns the memory that the dynamic variable occupied to the freestore.
- The freed memory can then be reused to create new dynamic variables.
- When you apply delete to a pointer variable, the dynamic variable to which it is pointing is destroyed. At that point, the value of the pointer variable is undefined, which means that you do not know where it is pointing.
- Note: C++ has no built-in test to check whether a pointer variable is a dangling pointer. One way to avoid dangling pointers is to set any dangling pointer variable equal to NULL.

  

```C++
delete ptr; // deletes a single variable
delete[] ptr; // deletes an array
```

  

  

### Automatic Variables

- **Local variables** are sometimes called **automatic variables** because their dynamic properties are controlled automatically for you. They are automatically created when the function in which they are declared is called and automatically destroyed when the function call ends.
- **Global variables** are sometimes **statically allocated variables** because they are truly static in contrast to dynamic and automatic variables.

  

  

### Call by Value and Call By Reference

- When a function is called, the values of the arguments are passed to the parameters, but the values of the parameters are not passed back to the arguments

  

```C++
    int num1 = 3;
    
    // does not manipulate num1
    cout << "Sum is " << addNUm(num1) << endl; 
	
		// num1 is still 3
    cout << num1;


int addNUm(int num)
{
    num += 3;
    return num;
}
```

![[/Untitled 31.png|Untitled 31.png]]

  

- In addition to calling by value, C++ supports calling by reference. By passing the address of a variable, both the calling function and the called function can reference the variable by its address, without the need to return a value.

![[/Untitled 1 17.png|Untitled 1 17.png]]

```C++
void addNum(int *num)
{
    *num += 3;


}

		
    int num1 = 3;
    
    int *num1Ptr = &num1;
    
    // pass a pointer
    addNum(num1Ptr);
		
		// num1 is directly manipulated
		// outputs 6
    cout << num1;
```

  

  

### Dynamic ARrays

- In C++, an array variable is actually a kind of pointer variable that points to the first indexed variable of the array.

```C++
int a[5];
int *ptr;
```

Since a is a pointer that points to a variable of type int (namely, the variable a[0]), the value of a can be assigned to the pointer variable p

```C++
p = a;
```

p points to the same memory location that a points to. Thus, p[0], p[1]…p[n] refers to the index variables a[0], a[1],…,a[n] for n=0-4.

  

The square bracket notation used for arrays applies to pointer variables as long as the pointer variable points to an array in memory.

  

### Array Variables and Pointer Variables

```C++
a = p2; // ILLEGAL: You cannot assign a different addr to a
```

- You cannot change the pointer value in an array variable
- The underlying reason is that an array variable is not of type *_int, but its type is a const version of *int._ An array variable, like a, is a pointer variable with the modifier const.

  

Pointers which contain for example, the beginning address of an array may undergo addition and subtraction, which will cause them to point to preceding or subsequent elements.

- pa is a pointer, pa+1 will not point to the byte 1 past pa, but instead to the data item one unit past pa.
- Therefore, the type of data item to which a pointer points must be declared.

  

```C++
    int *ptr;
    int size, counter;

    cout << "Enter the size of the array: ";
    cin >> size;

    ptr = new int[size];

cout << "Enter " << size << " elements: \n";

    

    // read input from the user
    for (int i=0; i<size; i++){
        // subscript notation
        cout << "Element[" << i << "]: ";
        cin >> ptr[i]; // same thing as cin >> *(ptr+i); // pointer notation
    }

    // output the array
    for (int i=0; i<size; i++) {
        cout << "Element [" << i << "]: " << *(ptr+i) << endl;
    }

    delete[] ptr;
```