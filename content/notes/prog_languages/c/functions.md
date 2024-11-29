+++
title = 'C Functions in stdlib'
date = 2024-02-11

+++

### fgets

- The standard function `fgets` can be used to read a **string** from the keyboard or file (usually text). The general form of an `fgets` call is:
- `fgets(buffer, n, fp);` Reads `n-1` characters from the file pointed to by fp and stores the string in buff. A `NUL` character `'\0'` is appended as the last character in buff.
- If `fgets` encounters a newline character or the end of file before `n-1` characters is reached, then only the characters up to that point are stored in buff.
- returns `NULL` if an error occurred.
- helps prevent **buffer overflow**

### fputs

- `fputs(str, fp)` writes str to the file pointed by fp (usually text). Doesn't add newline

### puts

- The return value of `puts()` is an `int`. It returns `EOF` if an error occurs, or a non-negative value if it's successful.
- adds newline

### scanf

- The function `scanf` is notorious for its poor end-of-line handling
- If the user enters wrong data, it can leave things on the input stream etc.

### sscanf

- The name `sscanf` stands for string`scanf`. It works on strings instead of the standard input.
- it takes as its first parameter the string from where it has to read its input

### fscanf

- `fscanf(fp, conversion_specifiers, vars)` Reads characters from the file pointed to by fp and assigns input to a list of variable pointers vars using **conversion_specifiers**

### fprintf

- `count = fprintf(file, format, paramter-1, paramter-2, ...);` The function `fprintf` converts data and writes it to a **file**.
- _file_ is a pointer to `FILE`
- _count_ is the number of characters sent, -1 if an error occurred.
- _format_ describes how the arguments are to be printed.
- _parameter-1, paramter-2,..._ are parameters to be converted and sent.

### sprintf

- `sprintf` is similar to `fprintf` except that the first argument is a string.

### vprintf

- The `vprintf()` function is similar to the `printf()` function. However, the variable values corresponding to the format specifiers in the format string are no longer passed as separate parameters after the format string, but instead a `va_list` variable is accepted, so that our variable argument list can be used instead.

### asprintf

- "allocated string print formatted"
- first paramter is a pointer to a string variable of type `char **`
- much safer than `sprintf` as it allocates the string dynamically to which it sends output.

### getline

- GNU specific extension
- reads an entire line from a stream, upto and including the next newline character
- takes threee paramters:
- a pointer to a block allocated with `malloc` or `calloc`
- a pointer to a variable of type `size_t`
- the stream from which to read the line.

### getdelim

- enables to specify other delimiter characters than newline
- same syntax as `getline` except the third paramter specifies the delimiter character, and the fourth paramter is the stream.

### getchar

- read a single character from stdin. no parameters. reads the next character as `unsigned char`

### putchar

- writes a single char on stdout. takes a single integer paramter containing a character.

### getc

- reads a single character from a stream other than stdin.
- accepts a stream argument from which to read.
- highly optimized, usually implemented as a macro function.

### ungetc

- steps the file position indicator back by one byte within the file and reverses the effect of the last character read operation

### putc

- writes a single character to a stream other than stdout
- accepts a stream argument to which to write.
- takes a single integer paramter containing a character.
- highly optimized, usually implemented as a macro function

### fgetc

- `fgetc(file_ptr)`
- returns the next character from the file pointed to by file_ptr. EOF is returned if end of file has been reached
- better for reading from a stream that is not interactively produced by a human.

### fputc

- `fputc(char, fp)` Writes character char to the file pointed to by fp.

### fread

- To read text or binary in blocks of fixed size.
- `read_size = fread(ptr, item_size, num_items, fp);` Reads `num_items` items of item_size size from the file pointed to by file pointer fp into memory pointed to by ptr.
- `read_size` is the size of the data that was read.

### fwrite

- used to write text or binary in blocks of fixed size
- `write_size = fwrite(ptr, item_size, num_itesm, fp)` writes num_items items of item_size from pointer ptr to the file pointed to by fp.

### ftell

- `ftell(fp)` Returns a long int value corresponding to the `fp` file pointer position in number of bytes from the start of the file.

### fseek

`fseek(fp, num_bytes, from_pos)` Moves the `fp` file pointer position by `num_bytes` bytes relative to position `from_pos`, which can be one of the following constants:

- `SEEK_SET` start of file
- `SEEK_CUR` current position
- `SEEK_END` end of file
- returns 0 if the operation was successful, or a nonzero value ow.

### fflush

- To ensure that the date read from or written to a fully-buffered stream shows up right away.
- Flushing moves the characters from the buffer to the file, if they haven't been already moved.
- pass the function the stream to flush
- returns 0 if successful or `EOF`

### feof

- `feof(fp)` Returns a nonzero value if the end of stream has been reached, 0 otherwise. feof also sets EOF.

### ferror

- `ferror(fp)` Returns a nonzero value if there is an error, 0 for no error
- to check the _error indicator_ for a stream.
- passing the stream to `clearerr` will set both the error and EOF indicators back to 0.

### fopen

- returns its stream (A high-level connection opened to a file).
- Streams are represented by variables of type `FILE *`

### fclose

- It will close the passed stream and break the connection to the corresponding file, first reading any buffered input and writing any buffered output.
- `fclose(fp)` : returns 0 if file closed successfully, else return `EOF`

### open

- it returns its file_descriptor (low-level connection opened to a file)
- Two ways to open unbuffered file:
- `file_descriptor = open(name, flags);`
- `file_descriptor = open(name, flags, mode);` where:
- `file_descriptor` is an integer that is used to identify the file for the read, write, and close calls. If it is < 0, an error occurred.
- `name` is the name of the file.
- `flags` are defined in `fcntl.h` header file.
- We can combine flags using the | operator.

### read

- read a block of information from a file - can be binary or text. if text no terminating newline is added.
- The format of the `read` system call is:
- `read_size = read(file_descriptor, buffer, size)` where:
- `read_size` is the number of the bytes read. 0 indicates EOF, and a negative number indicates an error. of type `ssize_t`
- `file_descriptor` is the file_descriptor from which data is to be read
- `buffer` in memory where the data read will be stored. It is of type `void *`, and can be an array or a chunk of space reserved with `malloc`
- `size` is of type `size_t`, specifies the # of bytes to read.
- check sytem variable `errno` for one of the following error conditions
- `EBADF` : file descriptor invalid or is not open for reading.
- `EIO` : hardware error

### close

- the close call `flag = close(file_descriptor)` closes the file. where `flag` is zero for success, negative for error. `file_descriptor` is the file_descriptor of an open file.

### remove

- The `remove()` function is used to remove the file.
- returns 0 if it is successful, and nonzero if an error occurs.

### popen

- first argument : string containing a shell command, such as `lpr`.
- second arguments: string containing `r` or `w`.
- returns a stream open for reading or writing
- if there is an error: returns a null pointer.

### pclose

- closes a pipe opened by `popen`
- accepts a single argument as the stream to close.
- returns: status code returned by the program called by `popen`