  

An exception is an occurrence of an undesirable situation that can be detected during program execution.

- Sometimes, we don’t want the program to terminate if it encounters an exception

  

### try/catch Block

- The statements that may generate an exception are placed in a try block.
- The try block also contains statements that should not be executed if an exception occurs. The try block is followed by one or more catch blocks. A catch block specifies the type of exception it can catch and contains an exception handler.

![[/Untitled 32.png|Untitled 32.png]]

- if no exception is thrown in a try block, all catch blocks associated with that try block are ignored and program execution resumes after the catch block.
- If an exception is thrown in a try block, the remaining statements in that try block are ignored. The program searches the catch blocks in the order they appear after the try block and looks for an appropriate exception handler. If the type of thrown exception matches the parameter type in one of the catch blocks, the code of that catch block executes, and the remaining catch blocks after this catch block are ignored.
- The last catch block that has an ellipsis is designed to catch any type of exception

  

```C++
catch (int x)
{
	// exception handling code
}
```

- The identifier x acts as a parameter. In fact, it is called a catch block parameter.
- The data type int specifies that this catch block can catch an exception of type int.
- A catch block can have at most one catch block parameter.

  

Essentially, the catch block parameter becomes a placeholder for the value thrown. In this case, x becomes a placeholder for any thrown value that is of type int. This way, we can handle it or do something with that value, it can be accessed via the catch block parameter.

  

Suppose in a catch block heading only the data type is specified, no catch block parameter. The thrown value then may not be accessible in the catch block exception-handling code.

  

  

### Throwing an Exception

- In order for an exception to occur in a try block and be caught, the exception must be thrown in the try block.

```C++
throw expression;
```

in which expression is a constant value, variable, or object. The object being thrown can be either a specific object or an anonymous object.

  

  

  

### ORDER OF CATCH BLOCKS

  

We should be careful about the order in which we list catch blocks following a try block.

- A catch block can catch either all exceptions of a specific type or all types of exceptions.
- Suppose that an exception occurs in a try and is caught by a catch block. The remaining catch blocks associated with that try block are then ignored.
- Usually the catch with an ellipsis is the last block