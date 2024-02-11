+++
title = 'C Notes'
date = 2024-02-11

+++


## A BOOK on C

### Chapter 1

- To seed the random-number generator before it gets used can be done with the following line: `srand(time(NULL));`
- The expression `a+++` can be interpreted as either `a++ + b` or `a + ++b`.The correct grouping is the first one. This is because the compiler groups the longest string as a token first, and so uses ++ instead of + as a first token.
- The integers produced by the function `rand()` all fall within the interval `[0, n]`, where `n` is system-dependent. In ANSI C, the value of `n` is given by the symbolic constant `RAND_MAX`, which is defined in the standard header file `stdlib.h`.
- On most C systems, the function call `time(NULL)` returns the number of elapsed seconds since 1 January 1970. The value used to seed the random-number generator determines where in the mixed-up order `rand()` will start to generate numbers.
- The proper way to time C code is with the use of the `clock()` function.
- The function `drand48()`, can be used to produce randomly distributed doubles in the range `[0, 1]` and the function `lrand48()` can be used to produce randomly distributed integers in the range `[0, 2<sup>31</sup> – 1]`.

### Chapter 3

- In C, variables of any integral type can be used to represent characters.
- `char` is equivalent to either `signed char` or `unsigned char`, depending on the compiler.
- `signed char` : -128 to 127
- `unsigned char` : 0 to 255
- Arithmetic on `unsigned` variables is performed modulo 2^wordsize
- 3.7f `float`
- 3.7L|l `long double`
- The _precision_ describes the number of significant decimal places that a floating value carries.
- The range describes the limits of the largest and smallest positive floating values tat can be represented in a variable of that type.
- The C language provides the `typedef` mechanism, which allows to explicitly associate a type with an identifier.
  `typedef char uppercase;`
  `typedef int INCHES, FEET;`
- The named identifiers can be used later to declare variables or functions in the same way ordinary types can be used. Thus,
  `uppercase 	u;`
  `INCHES	length, width;`
- C provides the unary operator `sizeof` to find the number of bytes needed to store an object. `sizeof(object)`
- An object can be a type such as `int` or `float`, or it can be an expression such as `a+ b`, or it can be an array or structure type.
- A `char` or `short`, either `signed` or `unsigned`, or an enumeration type can be used in any expression where an `int` or `unsigned int` may be used.
- If all the values of the original type can be represented by an `int`, then the value is converted to an `int`; otherwise it is converted to an `unsigned int`. This is called **integral promotion**.
- The functions `printf()` and `scanf()` use the conversion characters and d, x, and o in conversion specifications for decimal, hexadecimal, and octal, respectively. With `printf()`, formats of the form `%x` and `%o` cause integers to be printed out in hexadecimal and octal notation but not prefaced with 0x or 0. The formats `%#x` and `%#o` can be used to get the prefixes.
- When using `scanf()` to read in a hexadecimal number, do not type an 0x prefix.
- The function `putchar()` returns the `int` value of the character that it writes.

### Chapter 5

- The function storage class specifier, if present, can be either `extern` or `static` but not both.
- The only storage class specifier that can occur in the parameter type list is `register`.
- If the expression passed as an argument to `assert()` is false, the system will print a message, and the program will be aborted.
- Every variable and function in C has two attributes: type and storage class.
- The four storage class are `auto` `extern` `register` `static`
- When a variable is declared outside a function, storage is permanently assigned to it, and its storage class is `extern`. Such a variable is considered to be global to all functions declared after it.
- Variables defined outside a function have external storage class, even if the keyword `extern` is not used.
- The keyword `extern` is used to tell the compiler to “look for it elsewhere, either in this file or in some other file.”
- `extern` variables can be used to transmit values across functions.
- All the functions have storage classes. This means we can use the word `extern`in function definitions and in function prototypes.
- The storage class `register` tells the compiler that the associated variables should be stored in high-speed memory registers, provided it is physically and semantically possible to do so.
- When speed is a concern, the programmer may choose a few variables that are most frequently accessed and declare them to be of storage class `register`. Common candidates for such treatment include loop variables and function parameters.
- The declaration `register i` is equivalent to `register int i;`
- A `register` declaration is taken only as advice to the computer.
- Static declarations have two important and distinct uses. The more elementary use is to allow a local variable to retain its previous value when the block is re-entered. The second and more subtle use is in connection with external declarations.
- Static external variables are scope-restricted external variables.
- Static functions are visible only within the file in which they are defined.
- In C, both external variables and static variables that are not explicitly initialized by the programmer are initialized to zero by the system.
- When declaring a variable in traditional C, it is permissible to write the storage class specifier and the type specifier in any order.
- In ANSI C, the storage class specifier is supposed to come first.

### Chapter 6

- It is good programming practice to define the size of an array as a symbolic constant.
- Arrays may be of storage class automatic, external, or static, but not register.
- If an external or static array is not initialized explicitly, then the system initializes all elements to zero by default.
- Automatic arrays are not necessarily initialized by system.
- Conversions during assignment between different pointer types usually are not allowed unless one of the types is a pointer to `void`, or the right side is the constant 0.
- Do not point at register variables.
- A pointer variable can take different addresses as values. In contrast, an array name is an address, or pointer, that is fixed.
- `double sum(double a[], int n)` is equivalent to function definition of `double sum(double *a, int n)`
- `malloc()` may take less time.

## IMT

- `%lf` rounds the number to nearest even
- `ptr[x]` means : current value of ptr + x elements more in the array
- `int * array[2] = {a, b}` points to two arrays a and b.
- `char words[3][10]` 3 words with max 10 characters (including `'\0'`)
- variables are stored in location called stack.
- dynamic memory is allocated in heap
- To the left of malloc(4) shows no. of bytes used up and how much space is remaining
- To the right of malloc(4) indicates how much free space is remaining in the heap.
- malloc reserves memory in multiples of 4.
- malloc(5) will reserve 8 bytes
- need to store the address where malloc allocated the memory.
- malloc returns a ptr that contains the address of that location where the block starts that malloc reserved.
- One should always cast malloc to the correct datatype.
- we must free the memory we allocated usin malloc using free(ptr_var)
- -> has higher precedence than &
- size is printed using format specifier `%zu`
- when we don't know number of elements for array, we can provide a pointer to the array and use malloc to allocate memory at runtime
- `polygon = (struct point *) malloc(num * sizeof(struct point))`
- `ptr->next` means dereferencing ptr and getting its value next
- curses.h allows the user to use a cursor in the terminal window
- menu.h allows the creation of menu, from which user can pick
- libcurses.so, libmenus.so
- `c` The `c` option is for only compiling, i.e not creating an executable.
* The difference between **const** and **#define** is that the former uses memory for storage and the latter does not.
* when a local variable is being passed out of a function, you need to declare it as static in the function.
* Pointers to functions, or **function pointers**, point to executable code for a function in memory. Function pointers can be stored in an array or passed as arguments to other functions.
* A function pointer declaration uses the * just as you would with any pointer: `return_type (*func_name)(parameters) `
* A function name points to the start of executable code, just as an array name points to its first element
* An array of function pointers can replace a switch or an if statement for choosing an action
* The statement `int (*op[4])(int, int)`; declares the array of function pointers
* A void pointer is used to refer to any address type in memory and has a declaration that looks like: `void *ptr;`
* When dereferencing a void pointer, you must **first type cast** the pointer to the appropriate data type before dereferencing with `*`.
* Void pointers are often used for function declarations.
* Another way to use a function pointer is to pass it as an argument to another function.
* A function pointer used as an argument is sometimes referred to as a **callback function** because the receiving function "calls it back"
* `(*st).age` is the same as `st->age`
* Also, when a `typedef` has been used to name the `struct`, then a pointer is declared using only the typedef name along with \* and the pointer name
* Arrays of structures are used for data structures such as linked lists, binary trees, and more.
* there is **statically managed data** in main memory for variables that persist for the lifetime of the program
* When allocating memory for a string pointer, you may want to use string length rather than the sizeof operator for calculating bytes
* When writing to a file, newline characters '\n' must be explicitly added.
* Use `errno`, `perror()`, and `strerror()` to identify errors through error codes.
* The `exit` command immediately stops the execution of a program and sends an exit code back to the calling process
* Some library functions, such as `fopen()`, set an error code when they do not execute as expected. The error code is set in a global variable named `errno`, which is defined in the **errno.h** header file. When using errno you should set it to 0 before calling a library function.
* To output the error code stored in errno, you use `fprintf` to print to the strerr file stream. Considered good programming practice.
* To use errno, you need to declare it with the statement `extern int errno;` at the top of your program (or you can include the errno.h header file).
* When a library function sets errno, a cryptic error number is assigned. For a more descriptive message about the error, you can use `perror()`. You can also obtain the message using `strerror()` in the `string.h` header file, which returns a pointer to the message text.
* `perror()` must include a string that will precede the actual error message
* There are more than a hundred error codes. We can use loop and strerror to list them
* Some of the mathematical functions in the math.h library set `errno` to the defined macro value `EDOM` when a domain is out of range. Similarly, the `ERANGE` macro value is used when there is a range error

## WIKIPEDIA

- When you declare a function or global variable as static, you cannot access the function or variable through the extern (see below) keyword from other files in your project. This is called _static linkage_.
- When you declare a local variable as static, it is created just like any other variable. However, when the variable goes out of scope (i.e. the block it was local to is finished) the variable stays in memory, retaining its value. The variable stays in memory until the program ends. While this behaviour resembles that of global variables, static variables still obey scope rules and therefore cannot be accessed outside of their scope. This is called _static storage duration_
- **volatile** is a special type of modifier which informs the compiler that the value of the variable may be changed by external entities other than the program itself
- `y = (x << shift) | (x >> (32 - shift));` 32 bit rotate instruction
- One use for the bitwise operators is to emulate bit flags. These flags can be set with OR, tested with AND, flipped with XOR, and cleared with AND NOT.
- To create a function that can accept a variable-length argument list, you must first include the standard library header stdarg.h. Next, declare the function as you would normally. Next, add as the last argument an ellipsis ("..."). This indicates to the compiler that a variable list of arguments is to followIf a function is to be called only from within the file in which it is declared, it is appropriate to declare it as a static function.
- eg `float average (int n_args, ...);`
- we must somehow, in the arguments, specify the number of elements in the variable-length part of the arguments
- When returning a pointer from a function, do not return a pointer that points to a value that is local to the function or that is a pointer to a function argument
- Declaring a typedef to a function pointer generally clarifies the code
- Function pointers can be useful for implementing a form of polymorphism in C.
- Function pointers may be used to create a strict member function
- `int (*pa)[ ]`; pointer 'pa' to an array of integer
- `int *ap`; array 'ap' of pointers to an integer
- `int *fp()`; function 'fp' Which returns a pointer to an integer
- sizeof has two variations:
- sizeof(type) : sizeof(int)
- sizeof expression : sizeof 1
- C defines that one element past end of array must be a valid address, i.e not cause a bus error or address error
- Using anything but the standard `*p++`, `(*p)++` causes more problems than it solves.
- Usually, more efficient to pass a pointer to the struct
- `char ***` is pointer to two dimensional jagged array

## King

- The `g` specifier is especially useful for displaying numbers whose size can't be predicted when the program is written or that tend to vary widely in size.
- In scanf, %d can only match an integer written in decimal form, while %i can match an integer expressed in octal, decimal, or hexadecimal
- C99 allows the use of the keyword `static` in the declaration of array parameters.

```C
int sum_array(int a[static 3], int n)
```

- The presences of `static` is merely a "hint" that may allow a C compiler to generate faster instructions for accessing the array. (If the compiler knows that an array will always have a certain min length, it can arrange to "prefetch" these elements)
- C supports three (and only three) forms of pointer arithmetic :
  - Adding an integer j to a pointer p : yields a pointer to hte element j places after the one that p points to. If p points to a[i], then p + j points to a[i+j] (provided a[i+j] exists)
  - Subtracting an integer from a pointer : if p points to the array element a[i], then p - j points to a[i-j].
  - Subtracting one pointer from another : if p points to a[i] and q points to a[j], then p-q is equal to i-j.
- For initializing 2D array using pointer arithmetic

```C
int *p;
for (p = &a[0][0]; p <= &a[NUM_ROWS-1][NUM_COLS-1]; p++)
    *p = 0;
```

- To clear row i of the array a. a[i] is a pointer to row i of the array a

```C
int a[NUM_ROWS][NUM_COLS], *p, i;
for (p = a[i]; p < a[i] + NUM_COLS; p++)
    *p = 0;
```

- A loop that clears column i of the array a, p is declared to be a pointer to an array of length NUM_COLS whose elements are integers.

```c
int a[NUM_ROWS][NUM_COLS], (*p)[NUM_COLS], i;
for (p = &a[0]; p < &a[NUM_ROWS]; p++)
    (*p)[i] = 0;
```

```C
for (p = a; p < a + NUM_ROWS; p++)
    (*p)[i] = 0;
```

- `gcc -E` to preprocess the code.

### Teaching C

- Some high-quality C code : [Musl](https://www.musl-libc.org/), [xV6](https://pdos.csail.mit.edu/6.828/2014/xv6.html)
- [Clang static analyzer](http://clang-analyzer.llvm.org/)
- [ASan](http://clang.llvm.org/docs/AddressSanitizer.html)
- [UBSan](http://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html)
- [MSan](http://clang.llvm.org/docs/MemorySanitizer.html)
- [tis-interpreter](http://trust-in-soft.com/tis-interpreter/)
- Coverage tool [gcov](https://gcc.gnu.org/onlinedocs/gcc/Gcov.html)

### Notes

- `gcc -O2 test.c` (number is the level of optimization)
- `clang -O1 test.c`
- 3 is the highest level of optimization.
- Be care of -Ofast (might cause side effect)
- unity, cmock.. for unit testing
- Use assert for testing
- strace shows system calls (requests made to the kernel)
- ltrace shows library calls
- brk and mmap are the main syscalls that are used for memory allocation on Linux.
- brk essentially moves the "Program break"
- Inspecting compiled binaries.
  - `strings a.out` : goes through the binary and prints all the printable characters.
  - `readelf --symbols a.out` : to look at the symbol table.
  - `readelf --segments a.out` : to look at individual segments
  - segments are blocks of memory that are gonna be loaded at the time the program runs.
  - sections are logical subpieces. Multiple sections are mapped to a particular segment
  - `objdump -t a.out` : gives symbol table
  - `objdump -s a.out` : to see different sections involved, .text : where code goes, .rodata : read only data
  - `objdump -d a.out` : to disassemble the binary
  - `objdump -x a.out` : to print every header information
  - `strip` : removes symbols from the binaries. to make binary smaller, other reason will be to avoid reverse engineering
- How to write static and shared libraries in C for Linux
- Using shared library : LD_LIBRARY_PATH="/path/to/libraries;$LD_LIBRARY_PATH" ./runtime_librarytest "Today is not the day"
- Using static library
- (N/d) \* d + (N % d) == N : for both positive as well as negative numbers.
- `make -d`
- `make --debug=b`
- `make -p`
- `pmap pid` : shows memory map of a process. It shows what memory addresses a process can access without seg faulting.
- Using macros for error checking. FAIL_IF, FAIL_IF_MSG
- Memory comes in pages of 4k. Some of those pages are shared between processes.
- You can measure cpu time, page faults, context switches with getrusage
- `ulimit` : get or set resource usage limits
- `ulimit -c` : we are talking about core dump limit
- `core` file is the snapshot of the memory at the instant it crashed. Helpful because we don't have to wait till the program executes and fails
- `gdb ./a.out core` : to start debugging at the core dump time
- Mutex (mutual exclusion) locks is a computing abstraction that allows one thread to basically exclude other thread, saying everybody wait I have the rights to this floor.
- If two processes or threads are working in parallel, that means they are actually doing the work at the same time. Parallelism typically requires some kind of hardware support like multiple cores or a co-processor
- The concept of concurrency is a little bit loser. Memory sharing can slow down the parallelism.
- If our computer uses little endian and we send bytes over the network to a computer that uses big endian then it will be interpreted differently. To resolve this we can use something called **Network Byte Order**. Currently Big Endian is the standard network byte order for the Internet.
- `man byteorder`
- `man htole32 htonl` : htonl (host to network long), ntohs (network to host short)
- binary protocol like DNS have to take byte order into account
- dlopen, dlsym, dlclose : for dynamic loading
- A shim is a piece of code that sits between a program and the libraries it uses. Shim intercepts the calls from the programs to the libraries.
- LD_PRELOAD=./shim.so ./randtest.out to make shim work. Load my library shim.so first. when looking for rand it will first look into this library and find the rand
- C++ does something called name mangling when you look the symbol table. The reason it does this is because it supports function overloading
- `-g3` : (all debug info)
- `-ggdb` : add any special gdb extensions
- designate a file and associate a block of our shared memory with that file.\
- setup shared memory functions : tfok, shmget, shmat, shmdt, shmctl
- "a.c" -> ftok -> key : just gets a numeric key associated with the file name that we are playing with.
- key -> shmget -> block : shmget uses that key to create or get a block of shared memory associated with that key. It's going to return a shared memory block id
- block -> shmat -> ptr : shmat uses the block id to map the block into the process's address space and give us a pointer to that block so we can actually start using the memory.
- Sometimes when using shared memory we need semaphores
- Semaphores belong to this category which we call synchronization primitives
- Other syncronization primitives include mutex locks, condition variables, monitors, barriers...
- A semaphore is basically an unsigned integer with some quirks. One of those quirks is that changes to the value of integer are atomic. Another quirk is we can only interact with semaphores using 2 operations : wait(), post() (also called signal)
- wait tries to decrement the value of the semaphore. If the value > 0, it succeeds and it returns. If the value == 0 then it waits until the value becomes positive again
- post just increments the value of the semaphore and returns
- Binary semaphores are used as mutex locks however there are some differences.
- `wait() // critical section post()`
- POSIX semaphore functions : sem_init, sem_open, sem_wait, sem_post, sem_unlink, sem_close
- System V semaphore fuctions : semget, semop, semctl
- inlining a function : rather than actually making the function call we are going to take the code for the function and just stick it inline in the generated code
- always_inline used in embedded code
- curses allows to do gui like thing in a text based interface
- `dot example.do -Tpdf > graph.pdf` to create a graph from dot file and it's pdf

### Pointers

- `int k` : on seeing the "int" part compiler sets aside 2 bytes of memory to hold the value of the integer. It also sets up a symbol table in which it adds that symbol **k** and the relative addess in memory where those 2 bytes were set aside.
- An "lvalue" of a variable is the value of its address, i.e. where it is stored in memory. The "rvalue" of a variable is the value stored in that variable (at that address)
- `*(*(multi + row) + col)` and `multi[row][col]` yield the same results
- `int (*p1d)[10];` : p1d here is a pointer to an array of 10 integers
- `int *p1d[10];` : p1d here is the name of an array of 10 pointers to type int

### Tools available for examining and formatting code

- **Cross references** These programs have names like `xref`, `cxref`, and `cross`. A cross reference prints out a list of variables and indicates where each variable is used.
- **Program indenters** Programs like `cb` and `indent` will take a program and indent it _correctly_
- **Pretty printers** A pretty printer such as `vgrind` or `cprint` will take the source and typeset it for printing on a laser printer.
- **Call graphs** `calls` produces call graphs. The call graphs show who calls whom and who is called by whom.
- Buffered I/O does not write immediately to the file. Instead, the data is kept in a buffer until there is enough for a big write, or until it is flushed.
- In unbuffered I/O, the data is immediately sent to the file. Each read or write requires a system call.
- Unbuffered I/O should be used only when reading or writing large amounts of binary data or when direct control of a device or file is required.
- In binary files, you usually put an identification number in the first four bytes of the file. This number is called the _magic number_.

### Error handling

- Signals are events raised by the host environment or operating system to indicate that a specific error or critical event has occurred
- To handle signals, a program needs to use the `signal.h` header file
- The `setjmp` function can be used to emulate the exception handling feature of other programming languages.
- The first call to `setjmp` provides a reference point to returning to a given function
- A call to `longjmp` causes the execution to return to the point of the associated `setjmp` call.
- it is generally preferred to use the return value of a function to indicate an error, if possible.

### Common Practices

- Some examples with higher dimensions
  - `int dim1[w];` dim1[i]
  - `int dim2[w*x];` dim2[w*j + i]
  - `int dim3[w*x*y];` dim3[w * (x*i+j) + k] index is k + w*j + w*x\*i
  - `int dim4[w*x*y*z];` dim4[w*(x*(y*i+j) + k) + l] index is w*x*y*i + w*x*j + w*k + l
- Similarly, if its to left to the client to destroy objects correctly, they may fail to do so, causing resource leaks. It is better to have an explicit destructor which is always used.
- After `free()` has been called on a pointer, it becomes a dangling pointer. One simple solution to this is to ensure that any pointer is set to a null pointer immediately after being freed.

### union

- A union is like a structure in which all of the members are stored at the same address. Only one member can be in a union at one time.
- The union data type was invented to prevent the computer from breaking its memory up into many inefficiently sized chunks, a condition that is called memory fragmentation.
- A union works because the space allocated for it is the space taken by its largest member
- One way to tell what type of member is currently stored in the union is to maintain a flag variable for each union.
- Unions are often used within structures because a structure can have a member to keep track of which union member stores a value.
- A union can also contain a structure.
- When you want to access the union members through a pointer, the -> operator is required.
- Arrays of unions allow storing values of **different types.**

### compilation

- The switch `-ansi` turns off features of GNU C that incompatible with ANSI C.
- The `-pedantic` switch causes the compiler to issue a warning for any non-ANSI feature it encounters.
- `gcc -E prog.c` runs the program through the preprocessor and generates the output.
- The compiler switch `-Dsymbol` allows symbols to be defined on the command line.
- clang compiles source files faster than gcc but gcc _sometimes_ generates faster code.
- **Optimizations**
  - `-O2` (general), `-O3` (sometimes): Test under both levels then keep the best performing binaries.
  - `-Os` helps if concern is cache efficiency
- **Warnings**
  - `-Wall -Wextra -Wpedantic`
  - during testing should add `-Werror` and `-Wshadow` on all platforms.
  - extra fancy options include `-Wstrict-overflow -fno-strict-aliasing`
  - as of now, clang reports some valid syntax as a warning, should add `-Wno-missing-field-initializers`
- **LTO - Link Time Optimization**
  - [clang LTO](http://llvm.org/docs/LinkTimeOptimization.html)
  - [guide](http://llvm.org/docs/GoldPlugin.html)
  - [gcc LTO](https://gcc.gnu.org/onlinedocs/gccint/LTO-Overview.html)
- clang and gcc releases support LTO by just adding `-flto` to command line options during object compilation and final library program linking.
- **Architecture**
  - `-march=native`
  - give the compiler permission to use your CPU's full feature set.

### Preprocessor

- The `#if` preprocessor directive is a lot more flexible than `#ifdef`. It allows for expressions combined using operators.
- There is also the `#elif` preprocessor directive, which works similar to an `else if ()` construct.
- The `#error` directive can be used to force a diagnostic message from the compiler.
- The `#pragma` directive can be used to control compiler specific features
- The `#line` directive can be used to change the current line number value.
- `#define`, `#undef` Defining and undefining macros.
- `#define` can also be used to create function-like macros with arguments that will be replaced by the preprocessor. Be cautious with this.
- `#ifdef`, `#ifndef`, `#if`, `#else`, `#elif`, `#endif` Conditional compilation.
- `#error`, `#warning` Output an error or warning message. An error halts compilation.
- The #ifdef, #ifndef, and #undef directives operate on macros created with #define.
- For example, there will be compilation problems if the same macro is defined twice, so you can check for this with an #ifdef directive. Or if you may want to redefine a macro, you first use #undef.
- The defined() preprocessor operator can be used with #if, as in: `#if !defined(LEVEL)`
- The C preprocessor provides the following operators.
- The # macro operator is called the **stringification** or **stringizing** operator and tells the preprocessor to convert a parameter to a string constant.
- The ## operator is also called the **token pasting** operator because it appends, or "pastes", tokens together

### What should you put in header files?

- External declarations of global variables and functions
- Preprocessor macro definitions
- Structure definitions
- Typedef declarations
- There are a few things _not_ to put in header files:
  - Defining instances of global variables.
  - Function bodies.

### Libraries

- Libraries specified earlier on the command line take precedence over those defined later, and code from later libraries is only linked in if it matches a reference (function definition, macro, global variable, etc.) that is still undefined
- There are two kinds of library: _static_ libraries and _shared_ libraries. When you link to a static library, the code for the entire library is merged with the object code for your program. If you link to many static libraries, your executable will be enormous.
- The file name for a library always starts with lib and ends with either `.a` (if it is static) or `.so` (if it is shared).
- If you want to link the static version of the library, you must use the GCC option _--static_ - `gcc -o math math.c -lm --static`

### Steps for Creating a Library

1. Write the source code in file `lpr_print.c`
2. To create a static library called `liblprprint.a` containing this function, just type the following two command lines:

```C
gcc -c lpr_print.c
ar rs liblprprint.a lpr_print.o
```

2. To create a shared library called `liblprprint.so` instead, enter the following sequence of commands:

```c
gcc -c -fpic lpr_print.c
gcc -shared -o liblprprint.so lpr_print.o
```

3. Create a header file that will allow users access to the functions in your library.
4. Put your libraries and include file somewhere code can access them. For the sake of this example, create the directories `include` and `lib` in home directory then move the `.a` and `.so` files to `lib`, and the `.h` file to `include`
5. To run a program linked to a shared version of the library type the following into shell `export LD_LIBRARY_PATH=~/lib:$LD_LIBRARY_PATH`
6. Write a program that uses the library

```C
#include <liblprprint.h>
int main()
{
  lpr_print("Hello, Multiverse!\nHow are ya? \n");
  return 0;
}
```

- To compile the program using the static library type:
  - `gcc --static -I~/include -L~/lib -o printer printer.c -llprprint`
- Using the `--static` option forces the compiler to link all libraries statically. To use the static version of a library and some shared versions of other libraries we can omit the `--static` option and specify the static version of the library explicitly as follows :
  - `gcc -I~/include -L~/lib -o printer printer.c ~/lib/liblprprint.a`

### Modern C

- For modern programs, use `#include <stdint.h>` then use _standard_ types.
- The common standard types are:
  - `int8_t`, `int16_t`, `int32_t`, `int64_t` - signed integers
  - `uint8_t`, `uint16_t`, `uint32_t`, `uint64_t` - unsigned integers
  - `float` - standard 32-bit floating point
  - `double` - standard 64-bit floating point
- Also have **fast** and **least** types defined in the `stdint.h`
- **Fast** types are:
  - `int_fast8_t`, `int_fast16_t`, `int_fast32_t`, `int_fast64_t` — signed integers
  - `uint_fast8_t`, `uint_fast16_t`, `uint_fast32_t`, `uint_fast64_t` — unsigned integers
- Fast types provide a minimum of `X` bits, but there is no guarantee the underlying storage size is exactly what you request
- **Least** types are:
  - `int_least8_t`, `int_least16_t`, `int_least32_t`, `int_least64_t` — signed integers
  - `uint_least8_t`, `uint_least16_t`, `uint_least32_t`, `uint_least64_t` — unsigned integers
- Least types provide with the most _compact_ number of bits for the type you request.
- The correct type for pointer math is `uintptr_t` defined by `stdint.h`, while the also useful `ptrdiff_t` is defined by `stddef.h`
- `ptrdiff_t` is the proper type for storing values of subtracted pointers.
- The current date and time is returned when we call the `time()` function with `NULL` as paramter.
- the `srand()` function initializes the rng. It takes an **unsigned** interger value as a parameter.
- The `rand()` function returns a random number between 0 and `RAND_MAX` (the value of `RAND_MAX` is defined in `stdlib.h`)
- you may not pass any pointers to `free()` other than those received from `malloc()`

### Stream buffering

- There are three main kinds of buffering to know about:
  - **No buffering:** When we write characters to an unbuffered stream, the operating system writes them to the file as soon as possible.
  - **Line buffering:** When we write characters to a line-buffered stream, the operating system writes them to the file when it encounters a newline character.
  - **Full buffering:** When you write characters to a fully-buffered stream, the operating system writes them to the file in blocks of arbitrary size. Most streams are fully-buffered when opened and this is the best solution.
- Streams connected to interactive devices such as terminal are line-buffered.

### Putting a program together

- The best option of all is to use the `argp` interface for processing options.
- The main interface to `argp` is the `argp_parse` function. Usually, the only argument-parsing code you will need in `main` is a call to this function. The first parameter it takes is of type `const struct argp *argp`, and specifies an `ARGP` structure
- The second parameter is simply `argc`, the third simply `argv`.
- The `argp_parse` returns a value of type `error_t`: usually either `0` for success, `ENOMEM` if a memory allocation error occurred, or `EINVAL` if an unknown option or argument was met with.
- [argp](http://www.crasseux.com/books/ctutorial/argp-description.html#argp%20description)
- [argp example](http://www.crasseux.com/books/ctutorial/argp-example.html#argp%20example)
- that `envp` is an array of strings, just as `argv` is. It consists of a list of the environment variables of your shell, in the following format: NAME=value
- the simplest way to access the value of an environment variable is with the `getenv` function, defined in the system header `stdlib.h`
- Do not modify strings returned from `getenv`; they are pointers to data that belongs to the system. To process a value returned from `getenv`, copy it to another string with `strcpy`. To change an environment variable from within program (not usually advisable), use the `putenv`, `setenv`, and `unsetenv` functions
- Whenever you're building or modifying strings, you have to make sure that the memory you're building or modifying them in is writable. That memory should either be an array you've allocated, or some memory which you've dynamically allocated.
- Function calls cannot be used as macro parameters.

### Modules, Separate Compilation, Make Files

- A module is also sometimes called a _library_, although the term library is used more for code that is in an _already-compiled_ form.
- **grades.h** is the _interface_ to the module. It contains _functional prototypes_. It may also contain things like _constants_ and _type definitions_
- **grades.c** This is the _implementation_ of the module.
- When you run the _make_ utility, it examines the _modification times_ of files and determines what needs to be regenerated. Files that are _older_ than the files they depend on must be regenerated. Regenerating one file may cause others to become _old_, so that several files end up being regenerated.
- For simple make files, there will be at least 2 parts:
  - A set of _variables_, which will specify things like the C compiler/linker to use, flags for the compiler, etc.
  - A set of _targets_, i.e., files that have to be generated.

#### Variables

- `CC = gcc`
- `CFLAGS = -pedantic -Wall -g`

#### Targets

- For each target, there are typically 1 or 2 lines in a make file. Those lines specify:
  1. its dependencies (easy to determine from a dependency chart)
  2. and possibly a command to generate the target (easy to determine from Knowledge of separate compilation)

### Strings as arrays, as pointers, and string.h

- **Strings as arrays:**
  - In C, the abstract idea of a string is implemented with just an array of characters. For example, `char label[] = "Single"`
  - The type of array allocation, where the size of the array is determined at compile-time, is called _static allocation_
- **Strings as pointers**
  - When we use a `char *` to keep track of a string, the character array containing the string must already exists.

### Pointer, Array, String Review

- The expression "\*br++" really means:
  1. Use what `br` points to (i.e. print out the `double`)
  2. Increment the pointer (i.e. make it point to the next `double`)

### Intro to Dynamic Allocation

- **Static allocation** is when the amount of space for an array or some other construct is determined at compile-time.
- **Array local variables must be of a static size**
- **Library routines for dynamic allocation**
  - C provide a set of routines for allocating space for _any type_ of object at run-time.
  - `ptr = malloc(bytes)`
  - `free(ptr)`

### Intro to File Input/Output in C

- The function `fscanf()`, like `scanf()`, normally returns the number of values it was able to read in.
- The bad thing about testing `EOF` is that if the file is not in the right format then `fscanf()` will not be able to read that line and it won't advance to the next line in the file.
- Another way to test for end of file is with the library function `feof()`. It just takes a file pointer and returns a true/false value based on whether we are at the end of the file.
- `static` function can only be called from other functions within the same source code file.

### Trie

- We use a **trie** to store pieces of data that have a _key_ (used to identify the data) and possibly a _value_ (which holds any additional data associated with the key)
- A trie shares prefixes that are common among keys.
- The first character of a string key in the trie is **always** at level 1.

### MIT C

- `gcc -g -O0 -Wall infilename.c -o outfilename.o` Embed debugging info and disable optimization
- Testing if atleast three of last four bits are on means the last four bits could have values : 0x7, 0xB, 0xD, 0xE, 0xF. So extract last four bits and test if `bits == 0x7 || bits == 0xB || bits >= 0xD`
- `static` keyword has two meanings, depending on where the static variable is declared.
- Outside a function, `static` variables/functions only visible within that file, not globally (cannot be `extern`'ed)
- Inside a function, `static` variables:
  - are still local to that function
  - are initialized only during program initialization
  - do not get reinitialized with each function call.
- One of the ways to build a complex program is to develop it iteratively, solving one problem at a time and testing it thoroughly.

## CSAPP

- When you read from an `int` pointer in an expression ``*myIntPtr` you get back the content of the location interpreted as a multi-byte value of type int. When you read from a `char` pointer in an expression `*myCharPtr`, you get back the content of the location interpreted as a single-byte value of type `char`.
