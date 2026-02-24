+++
title = 'Understanding Pointers'
date = 2024-02-11

+++

### Chap 1

- When a C program is compiled, it works with three types of memory:
  - Static/Global
  - Automatic
  - Dynamic
- [Undefined behaviour](https://wiki.sei.cmu.edu/confluence/display/c/DD.+Unspecified+Behavior)
- If a pointer is declared as global or static, it is initialized to NULL when the program starts.
- `size_t` created to provide a safe type for sizes. `%zu` or `%lu`
- `ptrdiff_t` created to handle pointer arithmetic
- `intptr_t` and `uintptr_t` used for storing pointer addresses.
- `const int *p` - pointer to a constant integer, i.e pointer reference can be modified but not the value it points to.
- `int *const p` - constant pointer to an integer, i.e pointer reference cannot be modified but the value it points to can be modified.

### Chap 2

- A common practice is to always assign NULL to a freed pointer.

```C
int *pi = malloc(5 * sizeof(int));
memset(pi, 0, 5 * sizeof(int));
```

- When two pointers reference the same location, it is referred to as _aliasing_.
- Resource Acquisition Is Initialization (RAII) that addresses the allocation and deallocation of resources in C++.
- The GNU extension uses a macro called RAII_VARIABLE. It declares a variable and associates with the variable:
  - A type
  - A function to execute when the variable is created
  - A function to execute when the variable goes out of scope.

### Chap 3

- The program stack is an area of memory that supports the execution of functions and is normally shared with the heap. That is, they share the same region of memory.
- The program stack tends to occupy the lower part of this region, while the heap uses the upper part.
- Stack frames hold the parameters and local variables of a function.
- A stack frame consists of several elements, including:
  - _Return address_: The address in the program where the function is to return upon completion.
  - _Storage for local data_: Memory allocated for local variables
  - _Storage for parameters_: Memory allocated for the function's parameters.
  - _Stack and base pointers_: Pointers used by the runtime system to manage the stack.
- Several problems can occur when returning a pointer from a function, including:
  - Returning an uninitialized pointer
  - Returning a pointer to an invalid address
  - Returning a pointer to a local variable
  - Returning a pointer but failing to free it.
- Returning a pointer to a local data is an easy mistake to make if you don't understand how the program stack works.
- static array must be declared with fixed size.
- When a pointer is passed to a function, it is passed by value. If we want to modify the original pointer and not the copy of the pointer, we need to pass it as a pointer to a pointer.
- A more realistic example of where the comparison of function pointers would be useful involves an array of function pointers that represent the steps of a task
- A pointer to one function can be case to another type.
- This should be done with care since the runtime system does not verify that parameters used by a function pointer are correct
- returning a pointer to a local variable is bad because the memory allocated to the local variable will be overwritten by subsequent function calls

### Chap 4

- In most situations, the array’s size must be passed so the array can be properly handled in a function
- A 2-D array is not to be confused with an array of pointers.

```C
pv = pv + 1;
vector = vector + 1; // syntax error
```

- the pointer pv is an lvalue. An lvalue must be capable of being modified. An array name such as vector is not an lvalue and cannot be modified.
- arr[i][j] = address of arr + (i _ size of row) + (j _ size of element)
- two approaches for allocating contiguous memory for a 2D:
  - The first technique allocates the “outer” array first and then all of the memory for the rows.
  - The second technique allocates all of the memory at once.

### Chap 5

- String declarations are supported in one of three ways: either as a literal, as an array of characters, or using a pointer to a character.
- When literals are defined they are frequently assigned to a literal pool. This area of memory holds the character sequences making up a string.
- GCC uses a `-fwritable-strings` option to turn off string pooling
- Attempting to initialize a pointer to a char with a character literal will not work. `char* prefix = '+'; // Illegal`
- Always allocate dedicated memory for the concatenation:
- Passing a string is simple enough. In the function call, use an expression that evaluates to the address of a char. In the parameter list, declare the parameter as a pointer to a char
- There are situations where we want a function to return a string initialized by the function.
- we need to decide whether we want to pass the function an empty buffer to
  be filled and returned by the function, or whether the buffer should be dynamically allocated by the function and then returned to us
- When a buffer is passed:
  - The buffer's address and its size must be passed.
  - The caller is responsible for deallocating the buffer.
  - The function normally returns a pointer to this buffer.
- When a function returns a string, it returns the address of the string. The main concern is to return a valid string address. To do this, we can return a reference to either:
  - A literal
  - Dynamically allocated memory
  - A local string variable
- Returning the address of a local string will be a problem since the memory will be corrupted when it is overwritten by another stack frame. This approach should be avoided;

### function pointers

- you can create pointers to functions too, and knowing a function's address in memory enables you to pass functions as parameters too, giving your functions the ability to switch among calling numerous functions.
- Function pointers allow to pass functions as a parameter to another function. This enables to give the latter function a choice of functions to call. That is, plug in a new function in place of an old one simply by passing a different parameter. This technique is sometimes called _indirection_ or _vectoring_.
- To initialize a function pointer set it to the name of a function
- You should not initialise pointers with a value when you declare them.
- `const char *answer_ptr = "Forty-Two"` the data pointed to by answer_ptr is a constant.
- `char *const name_ptr = "Test"` constant pointer that cannot be changed.

### Strings

- String variables are either arrays of type `char` or have the type "pointer to `char`", that is, `char *`
- A _string array_ is an array of strings, which, of course, are themselves arrays of characters; in effect, a string array is a two-dimensional character array.
- String array is declared `char *menu[]`. This method of defining a two-dimensional string array is a combination of `char[]` and `*char` for initialising strings.
- Why not `realloc`
  - `realloc` can leave stale pointers.
  - `realloc` can introduce unexpected time delays.
