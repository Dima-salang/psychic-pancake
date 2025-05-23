  

# C-Strings (Character Arrays)

  

> [!important] Character array: an array whose components are of type char.

  

The most widely used character sets are ASCII and EBCDIC. The first character in the ASCII char set is the null character, which is nonprintable. Also, recall that in C++, the null char is represented as ‘\0’, a backslash followed by a zero.

  

Because the collating sequence of the null char is 0, the null char is less than any other char in the char set.

  

The most commonly used term for character arrays is c-strings. However, there is a subtle difference between char arrays and c-strings. In C++, c-strings are null terminated. A char array might not contain the null character, but the last char in a c-string is always the null char. Also, c-strings are stored in 1-D char arrays.

- “John L. Johnson”
- “Hello there”

  

From the definition of c-strings, there is a distinction between ‘A’ and “A”. The first is a char. “A” represents two chars: the char ‘A’ and ‘\0’. Similarly “Hello” represents six chars. To store ‘A’, we only need one memory cell of type char; to store “A”, we need two memory cells of type char one for ‘A’ and the other for the null char.

  

```JavaScript
char name[16];
```

- Because c-strings are null terminated and name has 16 components, the largest string that can be stored in name is of length 15, to leave room for the terminating ‘\0’. If you store a c-string of length 10 in name, the first 11 components of name are used and the last 5 are left unused.

  

```JavaScript
char name[16] = {'J', 'o', 'h', 'n', '\0'};
```

- declares an array name containing 16 components of type char and stores the c-string “John” in it. During char array variable declaration, C++ also allows the c-string notation to be used in the initialization statement. The above is equivalent to:

```JavaScript
char name[16] = "John";
```

```JavaScript
char name[] = "John";
```

  

Consider this:

```JavaScript
char studentName[26];
```

- Because aggregate operations such as assignment and comparison are not allowed on arrays, this statement is not legal.

```JavaScript
studentName = "List L. Johnson";
```

  

C++ provides a set of functions that can be used for c-string manipulation. The header file cstring defines these functions.

![[/Untitled 30.png|Untitled 30.png]]

  

## String Comparison

- In C++, c-strings are compared char by char using the system’s collating sequence.
    
    - “Air” is less than the C-string “Boat” because the first char of “Air” is less than the first char of “Boat”.
    
      
    

strlen returns the number of actual chars enclosed in quotation marks. It does not return the physical number of chars in memory.

  

# Reading and Writing Strings

- The one place where C++ allows aggregate operations on arrays is the input and output of c-string.

```JavaScript
char name[31];
```

## String Input

```JavaScript
cin >> name;
```

- stores the next input c-string into name. The length of the input c-string must be less than or equal to 30. If the length of the input string is 4, the computer stores the four chars that are input and the null char. If the length of the input c-string is more than 30, then because there is no check on the array index bounds, the computer continues storing the string in whatever memory cells follow name. This can corrupt memory cells.
- However, the extraction operator >> skips all reading whitespace chars and stops reading data into the current variable as soon as it finds the first whitespace char or invalid data. As a result, c-strings that contains blanks cannot be read using the >>. For this, we use the get function.

  

To read c-strings:

```JavaScript
cin.get(str, m+1);
```

- stores the next m chars or all chars until thew newline char ‘\n’ is found, into str. Thew newline char is not stored in str. If the input c-string has fewer than m chars, then the reading stops at the newline char which is inputted by pressing ENTER.

  

```JavaScript
char str[31];
cin.get(str, 31);
```

- If the input is WIlliam T. Johnson, then “William T. Johnson” is stored in str.
- What about Hello there, My name is Mickey Blair.
    - This is str with a len of 37. Because str can store at most, 30 chars, the c-string “Hello there, My name is Mickey” is stored into str.

  

Suppose we have

```JavaScript
char str1[26];
char str2[26];
char discard;
```

- Now the newline character remains in the input buffer and must be manually discarded. Therefore, you must read and discard the newline char at the end of the first line to store the second line into str2.

```JavaScript
cin.get(str1, 26);
cin.get(discard);
cin.get(str2,26);
```

  

To read an store a line of input, including whitespace chars, we can use getline.

```JavaScript
char textLine[100];
cin.getline(textLine, 100);
```

- will read and store the next 99 chars or until the newline char into textLine. The null char will be automatically appended as the last char of textLine.

  

  

## String Output

```JavaScript
cout << name;
```

- we can also use aggregate operations on outputting c-strings. The insertion operator << continues to write the contents of name until it finds the null char. If name does not contain the null char, then you will see strange output because it will continue to output data from memory adjacent to name until ‘\0’ is found.

  

  

# The string class

  

string class permits string literals

  

String literal - any sequence of chars enclosed in double quotes; also called string value, string, constant

  

string header file is required to use the string class

  

getline continuously accepts and stores chars from the keyboard until the terminating key is pressed. If the last argument is omitted, the Enter key will terminate their input.

```JavaScript
getline(cin, strObj, terminatingChar);
```

- using cin and getline inputs together in the same program may cause problems
- Cin accepts the input but leaves the newline code from the Enter key in the buffer, which will be picked up by the getline as the end of its. We can use cin.ignore() after cin.get() or cin.getline()

  

  

string class provides many methods for manipulating strings, including

- length
- at - returns the char at the specified index
- compare - compares two strings
- empty
- erase - removes chars from the string
- find - returns the index of the first occurrence of a string in an object
- insert - inserts one string into another string at the specified index
- replace - replaces chars in an object at the specified index
- substr - returns a portion of a string
- swap - swaps chars in a string with a specified object

- Concaternation operator + combines two strings
- All relational operators may be used to compare strings; ASCII or UNICODE code values are used for comparison.

  

## Character Manipulation Methods

- Character manipulation methods require the header file string or cctype
- character class provides methods for character manipulation, including
    - character type detection: isalpha, isalnum, isdigit, isspace, isprint, isascii, isctrl, ispunct, isgraph, isupper, slower
    - Char case conversion: toupper, tolower