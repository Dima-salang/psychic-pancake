A data type is called **simple** if variables of that type can store only one value at a time.

  

A **structured** data type is one in which each data item is a collection of other data items.

  

  

> [!important] An
> 
> **array** is a collection of a fixed number of components (also called **elements**) all of the same data type and in contiguous memory space.

  

### One-Dimensional Array

- is an array in which the components are arranged in a list form.

  

```C++
dataType arrayName[intExp];
```

- in which intExp specifies the number of components in the array and can be any constant expression that evaluates to a positive integer.

![[/Untitled 33.png|Untitled 33.png]]

### Accessing Array Elements

- arrayName[indexExp]
- in which indexExp, called the **index**, is any expression whose value is a nonnegative integer. The index value specifies the position of the component in the array.
- In C++, [] is an operator called the **array subscripting operator**.
- The array index starts at 0.

```HTML
int arrNums[10];
arrNums[5] = 34;
```

![[/Untitled 1 18.png|Untitled 1 18.png]]

  

It follows that array elements are individually separate variables that can be used just as any other variable, and that arrNums[0] is the name of an individual variable within the array.

  

Note:

```HTML
int arraySize;

cout << "Enter the size of the array: ";
cin >> arraySize;
cout << endl;

int list[arraySize];
```

Line 5 is not allowed. When the compiler compiles Line 1, the value of the variable arraySize is unknown. Thus, when the compiler compiles Line 5, the size of the array is unknown and the compiler will not know how much memory space to allocate for the array.

  

  

### Processing One-Dimensional Arrays

- We need an ability to step through the elements of the array. This is accomplished using a loop.
- Some basic operations performed on a 1-D array are:
    - Initializing
    - Inputting data
    - Outputting data stored in an array
    - Finding the largest and/or smallest element

  

```C++
/* ACCESSING ELEMENTS IN ARRAY */
int list1[100];
for (int i=0; i<100; i++){
	cout << list1[i];
}


/* INPUTTING DATA TO ARRAY */
int list[100];

for (int i=0; i < 100; i++) {
	cin >> list[i];
}

```

  

```C++
double sales[10];
double sum, largestSales, aveSales, largestIndex;

/* INITIALIZING ELEMENTS IN ARRAY */
for (int i=0; i<10; i++) {
	sales[i] = 0.0; // put initial values for the array
}

/* READING ELEMENTS INTO AN ARRAY */
for (int i=0; i<10; i++) {
	cin >> sales[i];
}

/* PRINTING AN ARRAY */
for (int i=0; i<10; i++){
	cout << sales[i] << " "; 
}

/* GETTING THE SUM OF THE ARRAY */
for (int i=0; i<10; i++) {
	sum = sum + sales[i];
}
aveSales = sum / 10;


/* GET THE LARGEST ELEMENT */
for (int i=0; i<10; i++) {
	if (sales[i] > largestSales) {
		largestSales = sales[i];
		largestIndex = i;
	}
}

/* GET THE SMALLEST ELEMENT */
int smallestIndex = 0;
for (int i=1; i<10; i++) {
	if (sales[smallestIndex] > sales[i]) {
		smallestIndex = i;
	}
}
cout << "MINIMUM SALES: " << sales[smallestIndex] << endl;
```

  

### Array Index Out of Bounds

  

```C++
double num[10];
int i;
```

  

The component num[i] is _valid,_ that is, i is a valid index if i = 0 to 9.

The index is **in bounds** if _index_ is between 0 and _ARRAY_SIZE - 1,_ that is, _0 ≤ index ≤ ARRAY_SIZE-1_. If _index_ is negative or _index_ is greater than _ARRAY_SIZE-1,_ then we say that the index is **out of bounds.**

  

Unfortunately, C++ does not check whether the index value within the range. If the index goes out of bounds and the program tries to access the components specified by the index, then whatever memory location is indicated by the index that location is accessed.

- This can result in altering or accessing the data of a memory location we should not modify, or in trying to access protected memory that causes the program to instantly halt.
- Several strange things can happen if the index goes out of bounds during runtime execution.
- It is solely the programmer’s responsibility to make sure that the index is within the bounds.

```C++
int list[10];
for (int i=0; i <= 10; i++){
	cout << list[i];
}
```

When i becomes 10, the loop test condition i ≤ 10 evaluates to true and the body of the loop executes, which results in storing 0 in list[10]. Logically, list[10] does not exist.

  

On some new compilers, if an array index goes out of bounds in a program, it is possible that the program terminates with an error message.

  

  

### Array Initialization during Declaration

```C++
double sales[5] = {1.0, 2.0, 3.0, 4.0, 5.0};
```

When initializing arrays as they are declared, it is not necessary to specify the size of the array. The size is determined by the number of initial values in the braces. However, you must include the brackets following the array name.

```C++
double sales[] = {1.0, 2.0, 3.0, 4.0, 5.0};
```

Although it is not necessary to specify the size of the array if it is initialized during declaration, it is a good practice to do so.

  

  

### Partial Initialization of Arrays During Declaration

When you declare and initialize an array simultaneously, you do not need to initialize all components of the array.

  

```C++
int list[10] = {0};
```

- declares list to be an array of 10 components and initializes all of the components to 0.

  

```C++
int list[10] = {8, 5, 12};
```

- Thus if all of the values are not specified in the initialization statement, the array components for which the values are not specified are initialized to 0.

```C++
int list[25] = {4, 7};
```

- declares list to be an array of 25 components. The first two components are initialized to 4 and 7, respectively, and all other components are initialized to 0.

```C++
int x[5] = {};
```

- Then some compilers may initialize each element of the array x to 0.

  

```C++
int list[10] = {2, 5, 6, , 8}; // illegal
```

- When you partially initialize an array, then all of the elements that follow the first uninitialized element must be uninitialized.
- In this initialization, because the fourth element is uninitialized, all elements that follow the fourth element must be left uninitialized.

  

  

### Some Restriction on Array Processing

```C++
int myList[5] = {0, 4, 8, 12, 16};
int yourList[5];

yourList = myList; // illegal
```

- This will generate a syntax error. C++ does not allow operations on an array. An **aggregate operation** on an array is any operation that manipulates the entire array as a single unit.

  

To copy one array into another array, you can do it component-wise using a loop.

```C++
for (int i=0; i<5; i++) {
	yourList[index] = myList[index];
}
```

  

Suppose you want to read data into the array yourList.

```C++
cin >> yourList; // illegal
```

- To read data into yourList, you must read one component at a time, using a loop.

```C++
for (int i=0; i<5; i++) {
	cin >> yourList[i];
}
```

  

Similarly, determining whether two arrays have the same elements and printing the contents of an array must be done component-wise. The following are legal in the sense that they do not generate a syntax error; however, they do not give the desired results.

```C++
cout << yourList;
if (myList <= yourList)
.
.
.
```

Solution:

```C++
for (int i=0; i<5; i++) {
	cout << myList[i] << "/t" << yourList[i] << endl;
	if (myList[i] != yourList[i]) {
		cout << "The two arrays are not equal..." << endl;
		break;
	}
}
```

  

  

### Arrays as Parameters to Functions

- In C++, arrays are passed by reference only.
- Because arrays are passed by reference only, you do not use the symbol & when declaring an array as a formal parameter.
- When declaring a 1-D array as a formal parameter, the size of the array is usually omitted. If you specify the size when it is declared as a formal parameter, the size is ignored by the compiler.

  

```C++
void funcArrayAsParam(int listOne[], double listTwo[])
{
	.
	.
	.
}
```

  

### Constant Arrays as Formal Parameters

- Recall that when a formal parameter is a reference parameter, then whenever the formal parameter changes, the actual parameter changes as well.
- You can still prevent the function from changing the actual parameter by using const.

  

```C++
void example(int x[], const int y[]){
...
}
```

The function example can modify the array x, but not the array y. Any attempt to change y results in a compile-time error. **it is a good programming practice to declare an array to be constant as a formal parameter if you do not want the function to modify the array.**

  

  

### Base Address of an Array and Array in Computer Memory

- The base address of an array is the address or memory location of the first array component or element.

  

```C++
int myList[5];
```

- This statement declares myList to be an array of five components of type int. The computer allocates five memory spaces, each large enough to store an int value for these components. Moreover, the five memory spaces are contiguous.
- The base address of myList is the address of the component myList[0]. Recall that main memory is an ordered sequence of cells and each cell has a unique address. Typically, each cell is one byte. Therefore, to store a value into myList[0], starting at the address 1000, the next four bytes are allocated for myList[0].

![[/Untitled 2 13.png|Untitled 2 13.png]]

- Now, myList is the name of an array. There is also a memory space associated with the identifier myList, and the base address of the array is stored in that memory space.

```C++
cout << myList << endl;

// OUTPUT 
0x14239ff660
```

- This will give the memory location of the first component or the base address of the array.

  

Then, the expression myList ≤ yourList evaluates to true if the base address of the array myList is less than the base address of the array yourList; and evaluates to false otherwise.

  

  

### FUNCTIONS CANNOT RETURN A VALUE OF THE TYPE ARRAY

- C++ does not allow functions to return a value of the type array.

  

  

### INTEGRAL DATA TYPE AND ARRAY INDICES

- Other than integers, C++ allows any integral type to be used as an array index. This can enhance a program’s readability

![[/Untitled 3 12.png|Untitled 3 12.png]]

  

  

### SEARCHING AN ITEM IN AN ARRAY

- Sequential search or linear search
    
    - Searching a list for a given item
    - Starting from the first array element
    - Compare searchItem with the elements in the array
    - Continue the search until either you find the item or no more data is left inthe list compare with searchItem.
    
      
    
      
    

  

### TWO AND MULTIDIMENTIONAL ARRAYS

- Two-dimensional array: collection of a fixed number of components or elements of the same type arranged in two dimensions
    - also called matrices or tables

```C++
dataType arrayName[intExp1][intExp2];
```

- where intExp1 and intExp2 are expressions yielding positive integer values, and specify the number of rows and the number of columns, respectively, in the array

![[/Untitled 4 9.png|Untitled 4 9.png]]

  

```C++
double sales[10][5];
```

- declares a two-dimensional array sales of 10 rows and 5 columns. The rows are numbered 0…9 and the columns are numbered 0…4

![[/Untitled 5 9.png|Untitled 5 9.png]]

  

  

### ACCESSING ARRAY COMPONENTS

- You need a pair of indices: one for the row position (which occurs first) and one for the column position (which occurs second).

```C++
arrayName[indexExp1][indexExp2];
```

  

### TWO-DIMENSIONAL ARRAY INITIALIZATION DURING DECLARATION

```C++
int board[4][3] = {{2, 3, 1},
									 {15, 25, 13},
									 {20, 4, 7}
									};
```

- Elements of each row are enclosed within braces and separated by commas
- All rows are enclosed within braces
- For number arrays, if all components of a row aren’t specified, unspecified ones are set to 0.

  

  

### PROCESSING TWO-DIMENSIONAL ARRAYS

- Ways to process a two-dimensional array:
    - Process the entire array
    - Process a particular row of the array, called row processing
    - Process a particular column of the array, called column processing
- Each row and each column of a two-dimensional array is a one-dimensional array
    - to process, use algorithms similar to processing one-dimensional arrays