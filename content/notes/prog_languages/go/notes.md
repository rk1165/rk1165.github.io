+++
title = 'Golang Notes'
date = 2024-02-11

+++

- To make it easier to debug and log maps, the formatting functions (like fmt.Println) always output maps with their keys in ascending sorted order
- Whenever a `for-range` loop encounters a multibyte rune in a string, it converts the UTF-8 representation into a single 32-bit number and assigns it to the value. The offset is incremented by the number of bytes in the rune. If the `for-range` loop encounters a byte that doesn’t represent a valid UTF-8 value, the Unicode replacement character (hex value 0xfffd) is returned instead.
- Using `:=` instead of `=` inside the closure creates a new `a` that ceases to exist when the closure exits.
- You can defer multiple functions in a Go function. They run in LIFO order; the last defer registered runs first.
- You can modify any element in the slice, but you can’t lengthen the slice.
- When returning values from a function, you should favor value types. Only use a pointer type as a return type if there is state within the data type that needs to be modified
- But if you are passing megabytes of data between functions, consider using a pointer even if the data is meant to be immutable.
- A slice of structs in Go has all of the data laid out sequentially in memory. This makes it fast to load and fast to process. A slice of pointers to structs (or structs whose fields are pointers) has its data scattered across RAM, making it far slower to read and process
- The Go runtime provides users a couple of settings to control the heap’s size. The first is the `GOGC` environment variable. The garbage collector looks at the heap size at the end of a garbage collection cycle and uses the formula `CURRENT_HEAP_SIZE + CURRENT_HEAP_SIZE*GOGC/100` to calculate the heap size that needs to be reached to trigger the next garbage collection cycle.
- By default `GOGC` is set to `100`, which means that the heap size that triggers the next collection is roughly double the heap size heap at the end of the current collection. Setting `GOGC` to a smaller value will decrease the target heap size and setting it to a larger value will increase it. As a rough estimate, doubling the value of `GOGC` will halve the amount of CPU time spent on GC.
- Setting `GOGC` to off disables garbage collection
- The second garbage collection setting specifies a limit on the total amount of memory your Go program is allowed to use
- The value for `GOMEMLIMIT` is specified in bytes, but you can optionally use the suffixes B, KiB, MiB, GiB, and TiB. For example, `GOMEMLIMIT=3GiB` sets the memory limit to 3 Gibibyte
- You should set `GOMEMLIMIT` below the absolute maximum amount of available memory so you have spare capacity
- The following rules help you determine when to use each kind of receiver:
  - If your method modifies the receiver, you must use a pointer receiver.
  - If your method needs to handle nil instances, then it must use a pointer receiver.
  - If your method doesn’t modify the receiver, you can use a value receiver.
- When you use a pointer receiver with a local variable that’s a value type, Go automatically takes the address of the local variable when calling the method.
- If you call a value receiver on a pointer variable, Go automatically dereferences the pointer when calling the method.
- Go considers both pointer and value receiver methods to be in the method set for a pointer instance. For a value instance, only the value receiver methods are in the method set
- it has `iota`, which lets you assign an increasing value to a set of constants.
- When using `iota`, the best practice is to first define a type based on `int` that will represent all of the valid values
- If you have a method on an embedded field that calls another method on the embedded field, and the containing struct has a method of the same name, the method on the embedded field is invoked, not the method on the containing struct
- Interfaces specify what callers need. The client code defines the interface to specify what functionality it requires.
- Your code should **Accept interface, return structs**. Accepting interfaces make your code more flexible and explicity declare what functionality is being used.
- The primary reason your functions should return concrete types is they make it easier to gradually update a function’s return values in new version of your code.
- One common use of `any` is as a placeholder for data for uncertain schema that’s read from an external source, like a JSON file.
- Type assertions can only be applied to interface types
- Go, however, allows methods on any user-defined type, including user-defined function types. This sounds like an academic corner case, but they are actually very useful. They allow functions to implement interfaces.
- If your single function is likely to depend on many other functions or other state that’s not specified in its input parameters, use an interface parameter and define a function type to bridge a function to the interface
- If you have an error condition that indicates a specific state has been reached in your application where no further processing is possible and no additional information needs to be used to explain the error state, a sentinel error is the correct choice.
- Sentinel errors are usually used to indicate that you cannot start or continue processing.
- Use `==` to test if the error was returned for sentinel errors.
- Sentinel errors begin with `Err`
- If you want to wrap an error with your custom error type, your error type needs to implement the method `Unwrap`.
- If you want to create a new error that contains the message from another error, but don’t want to wrap it, use `fmt.Errorf` to create an error, but use the `%v` verb instead of `%w`:
- To merge multiple errors into a single error use `errors.Join` function.
- Another way to merge multiple errors is to pass multiple `%w` verbs to `fmt.Errorf`:
- To check if the returned error or any errors that it wraps match a specific sentinel error instance, use `errors.Is`
- The `errors.As` function allows you to check if a returned error (or any error it wraps) matches a specific type
- Use `errors.Is` when you are looking for a specific instance or specific values. - - Use `errors.As` when you are looking for a specific type.
- It’s very rare that you want to keep your program running after a panic occurs
- When you run `go get` and pass it a module name, it doesn’t check your source code to see if the module you specified is used within your main module. Just to be safe, it adds an indirect comment. To fix this run `go mod tidy`
- `go list -m -versions github.com/learning-go-book-2e/simpletax` To list the versions of a dependency.
- The patch version number is incremented when fixing a bug, the minor version number is incremented (and the patch version is set back to 0) when a new, backward-compatible feature is added, and the major version number is incremented (and minor and patch are set back to 0) when making a change that breaks backward compatibility.
- The command `go mod graph` shows the dependency graph of your module and all of its dependencies
- `go get -u=patch github.com/learning-go-book-2e/simpletax` : to upgrade the bug patch release for the current minor version
- A workspace allows you to have multiple modules downloaded to your computer and references between those modules will automatically resolve to the local source code instead of the code hosted in your repository.
- The `go.work` file is meant for your local computer only. Do not commit it to source control!
- tools written in Go can also be built from source and installed on your computer via the `go install` command.
- `goimports -l -w .` : `-l` for listing files with incorrect formatting and `-w` for fixing it
- You can embed the contents of the files within your Go binary using `go:embed` comment
- you can only embed into a package-level variable. The variable must be of type `string`, `[]byte`, or `embed.FS`. If you have a single file, it’s simplest to use `string` or `[]byte`
- to put `/*` after the name of a directory you want to embed will include all hidden files within the root directory, but it will not include hidden files in its subdirectories.
- To include all hidden files in all subdirectories, put `all:` before the name of the directory.
- When you run `go generate`, it looks for specially formatted comments in your source code and runs programs specified in those comments. While you could use `go generate` to run anything at all, it is most commonly used by developers to run tools that generate source code using `go generate ./...`
- The `stringer` tool is used with `go generate` to add a `String` method to your enumeration’s values so they can be printed.
- By default channels are _unbuffered_. Every write to an open, unbuffered channel causes the writing goroutine to pause until another goroutine reads from the same channel. Likewise, a read from an open, unbuffered channel causes the reading goroutine to pause until another goroutine writes to the same channel.
- Go also has _buffered_ channels. These channels buffer a limited number of writes without blocking. If the buffer fills before there are any reads from the channel, a subsequent write to the channel pauses the writing goroutine until the channel is read. Just as writing to a channel with a full buffer blocks, reading from a channel with an empty buffer also blocks.
- Most of the time, you should use unbuffered channels.
- Any time you are reading from a channel that might be closed, use the comma ok idiom to ensure that the channel is still open.
- The responsibility for closing a channel lies with the goroutine that writes to the channel. Be aware that closing a channel is only required if there is a goroutine waiting for the channel to close (such as one using a for-range loop to read from the channel)
- The `select` keyword allows a goroutine to read from or write to one of a set of multiple channels.
- Each `case` in a `select` is a read or a write to a channel. If a read or write is possible for a `case`, it is executed along with the body of the `case`
- What happens if multiple cases have channels that can be read or written? The `select` algorithm is simple: it picks randomly from any of its cases that can go forward; order is unimportant.
- Just like `switch` statements, a `select` statement can have a `default` clause. Also just like `switch`, `default` is selected when there are no cases with channels that can be read or written.
- If you want to implement a nonblocking read or write on a channel, use a `select` with a `default`. The following doesn't wait if there's no value to read in `ch`; it immediately executes the body of `default`

```go
select {
    case v := <-ch:
        fmt.Println("read from ch:", v)
    default:
        fmt.Println("no value written to ch")
}
```
- Having a `default` case inside a `for-select` loop is almost always the wrong thing to do.
- Practically, you should never expose channels or mutexes in your API’s types, functions, and methods. It means they shouldn't be exported
- Any time a closure uses a variable whose value might change, use a parameter to pass a copy of the current value of the variable into the closure.
-  Proper use of a buffered channel means that you must handle the case where the buffer is full and your writing goroutine blocks waiting for a reading goroutine. 
- Buffered channels are useful when you know how many goroutines you have launched, want to limit the number of goroutines you will launch, or want to limit the amount of work that is queued up.
- Buffered channels work great when you want to either gather data back from a set of goroutines that you have launched or when you want to limit concurrent usage.
- They are also helpful for managing the amount of work a system has queued up.
- you can use a `nil` channel to disable a `case` in a `select`. When you detect that a channel has been closed, set the channel’s variable to `nil`. The associated `case` will no longer run, because the read from `nil` channel never returns a value.
- all you need to know is that reaching the timeout cancels the context.
- The `Done` method on the context returns a channel that returns a value when the context is canceled by either timing out or when the context’s cancel method is called.
- While `WaitGroups` are handy, they shouldn’t be your first choice when coordinating goroutines. Use them only when you have something to clean up (like closing a channel they all write to) after all of your worker goroutines exit.
- `errgroup` contains a type, `errgroup.Group`. It builds on top of `WaitGroup` to create a set of goroutines that stop processing when one of them returns an error.
- Common case for using mutex is when your goroutines read or write a shared value, but don’t process the value.
- In general, be careful when holding a lock while making a function call, because you don’t know what locks are going to be acquired in those calls
- Like `sync.WaitGroup` and `sync.Once`, mutexes must never be copied. If they are passed to a function or accessed as a field on a struct, it must be via a pointer. If a mutex is copied, its lock won’t be shared.
- First, call `Header` to get an instance of `http.Header` and set any response headers you need.
- Next, call `WriteHeader` with the HTTP status code for your response
- Finally, call the `Write` method to set the body for the response
- The *middleware* pattern uses a function that takes in an `http.Handler` instance and returns an `http.Handler`.
- We use the middleware pattern for cross-cutting concerns.
- There is another Go convention that the context is explicitly passed through your program as the first parameter of a function. The usual name for the context parameter is `ctx`.
- There are two context-related methods on `http.Request`:
  - `Context` returns the `context.Context` associated with the request.
  - `WithContext` takes in a `context.Context` and returns a new `http.Request` with the old request’s state combined with the supplied `context.Context`.
- When making an HTTP call from your application to another HTTP service, use the `NewRequestWithContext` function.
- There is a factory method for putting values into the context, `context.WithValue`. It takes in three values: a context, a key to look up the value, and the value itself
- The `Value` method on `context.Context` checks if a value is in a context or any of its parent contexts.
- The key for context value must be comparable
- There are two patterns used to guarantee that a key is unique and comparable. 
  - The first is to create a new, unexported type for the key, based on an `int`: `type userKey int`
  - Another option is to define the unexported key type using an empty struct: `type userKey struct{}`
- If you have a set of related keys for storing different values in the context, use the `int` and `iota` technique. If you only have a single key, either is fine 
- You can use one of two different functions to create a time-limited context. 
  - The first is `context.WithTimeout`. It takes two parameters, an existing context and `time.Duration` that specifies the duration until the context automatically cancels.
  - The second function is `context.WithDeadline`. This function takes in an existing context and a `time.Time` that specifies the time when the context is automatically canceled.
- If you want to find out when a context will automatically cancel, use the `Deadline` method on `context.Context`.
- There are two situations where you should think about handling cancellation. 
  - The first is when you have a function that reads or writes channels using a `select` statement. Include a `case` that checks the channel returned by the `Done` method on the context.
  - The second situation is when you write code that runs long enough that it should be interrupted by a context cancellation. Check the status of the context periodically using `context.cause`
- There are two common situations where `TestMain` is useful:
  - When you need to set up data in an external repository, such as a database
  - When the code being tested depends on package-level variables that need to be initialized
- The `Cleanup` method on `*testing.T` is used to clean up temporary resources created for a single test.
- To help you test your environment variable-parsing code, Go provides a helper method on `testing.T`. Call `t.Setenv()` to register a value for an environment variable for your test. 
- A fuzz test can allocate (or attempt to allocate) many gigabytes of memory and it might write several gigabytes of data to your local disk.
- In Go, benchmarks are functions in your test files that start with the word `Benchmark` and take in a single parameter of type `*testing.B`.
- We run a benchmark by passing the `-bench` flag to `go test`. This flag expects a regular expression to describe the name of the benchmarks to run. Use `-bench=.` to run all benchmarks. A second flag, `-benchmem`, includes memory allocation information in the benchmark outpu
- There are two patterns for testing code that depends on large interfaces.
  - The first is to embed the interface in a struct.
  - A better solution is to create a stub struct that proxies method calls to function fields. For each method defined on `Entities`, you define a function field with a matching signature on your stub struct.
-  a _stub_ returns a canned value for a given input, whereas a _mock_ validates that a set of calls happen in the expected order with the expected inputs.
- If you define a struct named `Foo`, the kind is `reflect.Struct` and the type is `Foo`
- Another thing that Go lets you do with reflection is create a function. You can use this technique to wrap existing functions with common functionality without writing repetitive code. Like adding timing to any function that's passed into it.
- The `unsafe.Pointer` type is a special type that exists for a single purpose: a pointer of any type can be converted to or from `unsafe.Pointer`. In addition to pointers, `unsafe.Pointer` can also be converted to or from a special integer type, called `uintptr`.
- You can sometimes achieve significant savings in memory usage simply by reordering the fields in heavily used structs in order to minimize the amount of padding needed for alignment.
- Run your code with the flag `-gcflags=-d=checkptr` to add additional checks at runtime.
- `select` can be challenging and lead to mistakes:
  - `default` is always active
  - a `nil` channel is always ignored
  - a full channel (for send) is skipped over
  - a *done* channel is *just another channel*
  - **available channels are selected at random**
- The `RuneCountInString(s string) (n int)` function of the utf8 package can be used to find the length of the string. This method takes a string as an argument and returns the number of runes in it.
- `len(s string)` returns number of bytes instead of string length. For instance, `Señor` has 6 bytes. 2 for `ñ` and 4 for others.
- For mutating strings we should use `s []rune` 