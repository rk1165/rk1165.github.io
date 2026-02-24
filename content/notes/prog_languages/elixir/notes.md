+++
title = 'Elixir Notes'
date = 2024-02-11

+++

- An atom is a constant whose name is its value. `:foo`
- The booleans `true` and `false` are also the atoms `:true` and `:false`, respectively.
- `is_atom(true)`, `is_boolean(:true)`, `:true === true`
- Names of modules in Elixir are also atoms. `MyApp.MyModule` is a valid atom.
- Atoms are also used to reference modules from Erlang libraries, including built in ones. `:crypto.strong_rand_bytes 3`
- `div(10, 5)`, `rem(10, 3)`
- Elixir provides the `||`, `&&`, and `!` boolean operators. These support any types:
  - `-20 || true` -> `-20`
  - `false || 42` -> `false`
  - `42 && true` -> `true`
  - `42 && nil` -> `nil`
- There are three additional operators whose first argument _must_ be a boolean
  - `true and 42` -> `42`
  - `false or true` -> `true`
  - `not false` -> `true`
- For strict comparison of integers and floats, use `===`.
  - `2 == 2.0` -> true
  - `2 === 2.0` -> false
- In Elixir any two types can be compared.
  - number < atom < reference < function < port < pid < tuple < map < list < bitstring
- String concatenation uses the `<>` operator

### Collections

- Accessing the list length will run in O(n)
- Tuples are similar to lists, but are stored contiguously in memory. Length access is fast but modification is expensive; the new tuple must be copied entirely to memory.
- `List.flatten([[[1], 2], [[[3]]]])` : `[1,2,3]`
- `List.foldl([1,2,3], "", fn value, acc -> "#{value}(#{acc})" end)` : "3(2(1()))"
- `List.foldr([1,2,3], "", fn value, acc -> "#{value}(#{acc})" end)` : "1(2(3()))"
- `List.replace_at([1,2,3], 2, "buckle my shoe")` : [1, 2, "buckle my shoe"], not a cheap operation
- Accessing tuples within lists : `List.keyfind(tuple_list, key, index, [default_value])`
- Delete tuple with key : `List.keydelete(tuple_list, key, index)`
- Replace a key : `List.keyreplace(tuple_list, :name, 0, {:first_name, "Dave"})`
- List subtraction is provided via the `--` operator. `["foo", :bar, 42] -- [42, "bar"]` -> `["foo", "bar"]`
- It is common for tuples to be used as a mechanism to return additional information from functions or pass options to functions.
- In Elixir, a keyword list is a special list of two-element tuples whose first element is an atom.
  - `[foo: "bar", hello: "world"]`
  - `[{:foo, "bar"}, {:hello, "world"}]`
- Keys are atoms. Keys are ordered. Keys do not have to be unique.
- Maps are the "go-to" key-value store. They allow keys of any type and are un-ordered.
  - `map = %{:foo => "bar", "hello" => :world}`
  - `map[:foo]`
  - `map["hello"]`
- `Map.keys map`
- `Map.values map`
- `map1 = Map.drop(map, [:where, :likes])`
- `map2 = Map.put(map, :also_likes, "Ruby")`
- `Map.has_key?(map1, :where)`
- `{value, updated_map} = Map.pop(map2, :also_likes)`
- `Map.equal?(map, updated_map)`
- Variables are allowed as map keys
- For maps containing only atom keys, there's a special syntax. `%{foo: "bar", hello: "world"}`
- There is a special syntax to use with atom keys: `map.hello`
- To update a map `%{map | foo: "baz"}`. This creates a new map. Only works if a key is already present.
- To create a new key use `Map.put/3`. `Map.put(map, :foo, "baz")`

### Enum

- All the collections, with the exception of tuples, are enumerables.
- For lazy enumeration use the `Stream` module
- `Enum.all?(["foo", "bar", "hello"], fn(s) -> String.length(s) == 3 end)` -> false
- `Enum.any?(["foo", "bar", "hello"], fn(s) -> String.length(s) == 5 end)` -> true
- To break collection up into smaller groups, `chunk_every/2` is the function
  - `Enum.chunk_every([1,2,3,4,5,6], 2)`
- To group our collection based on something other than size, we can use `chunk_by/2`
  - `Enum.chunk_by(["one", "two", "three", "four", "five"], fn(s) -> String.length(x) end)`
- `map_every/3` can be very useful to hit every `nth` item, always hitting the first one.
  - `Enum.map_every([1,2,3,4,5,6,7,8], 3, fn x -> x + 1000 end)`
- To iterate over a collection without producing a new value use `each/2`
- `Enum.min([5,3,0,-1])`
- `Enum.max([5,3,0,-1])`
- For convenience, `sort/2` allows us to pass `:asc` or `:desc` as the sorting function.
  `Enum.sort([2,3,1], :desc)`
- `uniq/1` to remove duplicates, `uniq_by/2` also removes duplicates, but it allows us to pass a function
- `Enum.map([1,2,3], fn number -> number + 3 end)`
- `Enum.map([1,2,3], &(&1 + 3))`

### Functions

- To define an anoymous function in Elixir we need the `fn` and `end` keywords.
  `sum = fn (a, b) -> a + b end`
- Using shorthand `sum = &(&1 + &2)`
- Pattern matching can be applied to function signatures as well.

```elixir
def call(param1, param2, param3) do
  IO.inspect(binding(), label: "Called with following bindings")
end
```

- We can use binding() to get a keyword list of all bound values
- Elixir pattern-matches the arguments that a function is called with against the arity the function is defined with.
- `alias`, `import`
- By default all functions and macros are imported but we can filter them using the `:only` and `:except` options
- `import List, only: [last: 1]`
- `import List, except: [last: 1]`
- In addition to the name/arity pairs there are two special atoms, `:functions` and `:macros`, which import only functions and macros respectively
  - `import List, only: :functions`
  - `import List, only: :macros`
- We could use `require` to tell Elixir you're going to use macros from another module.
- With the `use` macro we can enable another module to modify our current module's definition.
- If you do not intend to document a module, consider annotating the module `false`, like so `@moduledoc false`
- **Codepoints** are just simple Unicode characters which are represented by one or more bytes, depending on UTF-8 encoding.
- **Graphemes** consits of multiple _codepoints_ that are rendered as a single character.
- `i` information about given data type
- For errors that are part of the regular operation of a function (e.g. a user entered a wrong type of date), a function returns `{:ok, result}` and `{:error, reason}` accordingly.
- For errors that are not part of normal operations (e.g. being unable to parse configuration data) you throw an exception.
- a second version of the function with a ! (example!/1) that returns the unwrapped result or raises an error.
- `setup` will be run before each test and `setup_all` once before the suite. It is expected they will return a tuple of `{:ok, state}`, the state will be available for our tests.

### Troubleshooting

- `Process.list()`
- `Process.info(hd(Process.list()))`
- reductions : amount of instructions executed in that processes. To find the process which is busiest we need to take the reduction counts.
- `Process.info(pid, :current_stacktrace)`
- `Process.info(pid, :messages)` : to see the messages in the mailbox
- `Process.exit(pid, :kill)`
- `Process.whereis(:process_name)` : returns the PID of the named process - :process_name
- `Process.unregister(:process_name)`
- `Process.registered()` : to get a list of all registered process
- `Process.info(self, :links)` : to inspect the current link set of the shell process.
- `sys.get_state(pid)` : to get the current state of any process.
- `sys.trace(pid, true)` : to see what happens behind the scenes when a client sends a synchronous call request to the server process start the trace.
- `sys.trace(pid, false)` : to turn off tracing
- `sys.get_status(pid)` : to get the full status of a process

### Testing
- When testing our applications there are often times we need to make requests to external services. We may even want to simulate different situations like unexpected server errors. This is where `bypass` can help up.

### Ecto
- With a "belongs to" relationship, we can leverage Ecto's `build_assoc/3` function
- For a "many-to-many" relationship, we can leverage Ecto Changesets and the `put_assoc/4` function.
- We can use the `Repo.get/3` function to fetch a record from the database given its ID
- We can also fetch records that meet a given criteria with the `Repo.get_by/3` function
- The `Ecto.Query` module provides us with the Query DSL which we can use to write queries to retrieve data from the application’s repository.
- Using an expression with a binding in `select:` part allows you to specify exactly the way selected fields will be returned
- We can execute queries that include join statements with the help of the Ecto.Query.join/5 function.

### Mnesia
- `alias :mnesia, as: Mnesia`
- `Mnesia.create_schema/1` initializes a new empty schema and passes in a Node List
- The function `Mnesia.start/0` is asynchronous. It starts the initialization of the existing tables and returns the `:ok` atom.
- In case we need to perform some actions on an existing table right after starting Mnesia, we need to call the `Mnesia.wait_for_tables/2` function. It will suspend the caller until the tables are initialized.
- The function `Mnesia.create_table/2` is used to create tables within our database.
-  An Mnesia transaction is a mechanism by which a series of database operations can be executed as one functional block.
- The `Mnesia.match_object/1` function returns all records that match the given pattern
- The `Mnesia.select/2` function allows you to specify a custom query using any operator or function
- we can use the `Mnesia.table_info/2` function to retrieve the current structure of the table and the `Mnesia.transform_table/3` function to transform it to the new structure.

### Programming Elixir

- Files ending in `.ex` are intended to be compiled into bytecodes and then run, whereas those ending in `.exs` are more like programs in scripting languages.
- You can also load a file as if you'd typed each line into IEx using `import_file`
- Elixir has regular-expression literals, written as `~r{regexp}` or `~r{regexp}opts`.
- A PID is a reference to a local or remote process, and a port is a reference to a resource that you'll be reading or writing.
- The function `make_ref` creates a globally unique reference.
- Elixir allows us to leave off the square brackets if a keyword list is the last argument in a function call.
- Although typically all the keys in a map are the same type, that isn't required.
- If the keys are atoms, you can also use a dot notation.
- The `Date` type holds a year, a month, a day, and a reference to the ruling calendar.
- The `Time` type contains an hour, a minute, a second, and fractions of a second.
- The `with` expression serves double duty. First, it allows you to define a local scope for variables. Second, it gives you some control over pattern-matching failures.
- The value of the `with` is the value of its `do` parameter
- If you use `<-` instead of `=` in a `with` expression, it performs a match, but if it fails it returns the value that couldn't be matched
- The `&` operator converts the expression that follows into a function. Inside that expression, the placeholders `&1` and `&2`, and so on correspond to the first, second, and subsequent parameters of the function. So `&(&1 + &2)` will be converted to `fn p1, p2 -> p1 + p2 end`
- There's a second form of the `&` function capture operator. You can give it the name and arity of an existing function, and it will return an anonymous function that calls it.
- You can pass multiple lines to `do:` by grouping them with parentheses.
- If we need to distinguish based on the argument types or on some test involving their values, use _guard clauses_. These are predicates that are attached to a function definition using one or more `when` keywords.
- When you define a named functions, you can give a default value to any of its parameters by using the syntax `param\\value`.
- Modules also act as wrappers for macros, structs, protocols, and other modules.
- The optional second parameter lets you control which functions or macros are imported. You write `only:` or `except:`, followed by a list of `name: arity` pairs. It is a good idea to use `import` in the smallest possible enclosing scope and to use `only:` to import just the functions you need.
- You can set the same attribute multiple times in a module.
- An Elixir single-quoted string is actually a list of individual character codes.
- Elixir pattern matching is recursive and we can match patterns inside patterns.
- A struct is just a module that wraps a limited form of map. The name of the module becoms the name of the map type.
- Inside the module, you use `defstruct` macro to define the struct's members.
- Structs also play a large role when implementing polymorphism.
- Elixir has a set of nested dictionary-access functions. One of these, `put_in`, lets us set a value in a nested structure. The other two nested access functions are `get_in` and `get_and_update_in`
- There's a cool trick that the dynamic versions of both `get_in` and `get_and_update_in` support - if you pass a function as a key, that function is invoked to return the corresponding values.
- The `Access` module provides a number of predefined functions to use a parameters to `get` and `get_and_update_in`
- The `all` and `at` functions only work on lists. `all` returns all elements in the list, while `at` returns the nth element.
- The `key` and `key!` functions work on dictionary types (maps and structs)
- Sets are implemented using the module `MapSet`
- Technically, things that can be iterated are said to implement the `Enumerable` protocol.
- How do we get the stream to start giving us results? Treat it as a collection and pass it to a function in the `Enum` module.
- `Stream.cycle` takes an enumerable and returns an infinite stream containing that enumerable's elements.
- `unfold` is quite useful - it is a general way of creating a potentially infinite stream of values where each value is some function of the previous state. The key is the generating function. Its general form is `fn state -> {stream_value, new_state} end`
- The third argument to `Stream.resource` takes the final accumulator value and does whatever is needed to deallocate the resource.
- `Collectable` allows you to build a collection by inserting elements into it.
- Ranges cannot have new entries added to them.
- We can inject the elements of a range into an empty list using `Enum.into 1..5 []`
- The `into:` option takes values that implement the `Collectable` protocol. These include lists, binaries, functions, maps, files, hash dicts, hash sets, and IO streams.
- We use `IO.write` for the multiline string because `puts` always appends a newline.
- The notation `?c` returns the integer code for the character `c`
- `byte size <<1,2,3>>` `bit_size <<1,2,3>>`
- You can store integers, floats, and other binaries in binaries
- The `String` module defines functions that work with double-quoted strings.
- Raise an exception with the `raise` function. At its simplest, you pass it a string and it generates an exception of type `RuntimeError`
- The trailing exclamation point means that the function will raise an exception on error, and that exception will be meaningful.
- `mix help taskname` for info on a particular task
- `formatter.exs` configuration used by the source code formatter.
- Elixir has a convention. Inside the `lib/` directory, create a subdirectory with the same name as the project (so we’d create the directory `lib/issues/`). This directory will contain the main source for our application, one module per file.
- To convert the body from a string, we call the `Poison.Parser.parse!` function
- By importing `ExUnit.CaptureIO`, we get access to the `capture_io` function. This runs the code passed to it but captures anything written to standard output, returning it as a string.
- Mix can package our code, along with its dependencies, into a single file that can be run on any Unix-based platform. This uses Erlang’s `escript` utility, which can run precompiled programs stored as a Zip archive.
- When `escript` runs a program, it looks in your `mix.exs` file for the option `escript`. This should return a keyword list of `escript` configuration settings. The most important of these is `main_module:`, which must be set to the name of a module containing a `main` function.
- `mix escript.build` to package our program
- You can choose to change the minimum log level at runtime by calling `Logger.configure`
- We can add a breakpoint to our source code using the `pry` function.
- A call to `binding` shows the local variables when debugging
- The second way to create a breakpoint doesn’t involve any code changes. Instead, you can use the `break!` command inside IEx to add a breakpoint on any public function.
- `doctest <<module>>`
- Built-in Types
  - Value types :
    - Arbitrary-sized integers
    - Floating-point numbers
    - Atoms
    - Ranges
    - Regular Expressions
    - Ranges and Regexps are debatable
  - System types:
    - PIDs and ports
    - References
  - Collection types
    - Tuples
    - Lists
    - Maps
    - Binaries
- `1 in [1,2,3,4]` membership
- `bin = << 1, 2 >>`
- `bin = << 3 :: size(2), 5 :: size(4), 1:: size(2)>>`
- `d1 = Date.new(2018, 12, 25)`
- `{:ok, t1} = Time.new(12, 34, 56)`

### Elixir in Action

- A scheduler is preemptive — it gives a small execution window to each process and then pauses it and runs another process. Because the execution window is small, a single long-running process can’t block the rest of the system
- More generally, the pipeline operator places the result of the previous call as the first argument of the next call.
- Another construct, `alias`, makes it possible to reference a module under a different name.
- Moreover, an attribute can be registered, which means it will be stored in the generated binary and can be accessed at runtime. Elixir registers some module attributes by default. For example, the attributes `@moduledoc` and `@doc` can be used to provide documentation for modules and functions
- To extract an element from the tuple, you can use the `Kernel.elem/2` function, which accepts a tuple and the zero-based index of the element.
- To modify an element of the tuple, you can use the Kernel.put_elem/3 function, which accepts a tuple, a zero-based index, and the new value of the field in the given position:
- To get an element of a list, you can use the `Enum.at/2` function which is an O(n) operation.
- You can check whether a list contains a particular element with the help of the `in` operator.
- List.replace_at/3 modifies the element at a certain position
- You can insert a new element at the specified position with the `List.insert_at` function
- You can also prepopulate a map with the `Map.new/1` function. The function takes an enumerable where each element is a tuple of size two (a pair):
- Sometimes you want to proceed only if the key is in the map, and raise an exception otherwise. This can be done with the `Map.fetch!/2` function:
- To store a new element to the map, you can use `Map.put/3`
- If the total size of all the values isn’t a multiple of 8, the binary is called a bitstring — a sequence of bits:
- You can also concatenate two binaries or bitstrings with the operator `<>`
- In general, you should prefer binary strings over character lists. You can convert a binary string to a character list version using the `String.to_charlist/1` function.
- To convert a character list to a binary string, you can use `List.to_string/1`
- A closure always captures a specific memory location. Rebinding a variable doesn't affect the previously defined lambda that references the same symbolic name.
- The ones most frequently used are Range, Keyword, MapSet, Date, Time, NaiveDateTime, and DateTime
- A _keyword_ list is a special case of a list, where each element is a two-element tuple, and the first element of each tuple is an atom. The second element can be of any type
- Keyword lists are most often useful for allowing clients to pass an arbitrary number of optional arguments.
- A keyword list can contain multiple values for the same key. In addition, you can control the ordering of keyword list elements — something that isn’t possible with maps
- Elixir has couple of modules for working with date and time types : `Date`, `Time`, `DateTime`, and `NaiveDateTime`.
- A date can be created with the `~D` sigil. `date = ~D[2018-01-31]`
- You can represent a time with the `~T` sigil, by providing hours, minutes, seconds, and microseconds `~T[12:30:10]`.
- You can also work with datetimes using the `NaiveDateTime` modules. The naive version can be created with the `~N` sigil.
- `~U[2000-10-01 12:30:10Z]` datetime. Z offset specifies the TZ offset. Z is Zero for UTC
- An IO list is a special sort of list that’s useful for incrementally building output that will be forwarded to an I/O device, such as a network or a file. Each element of an IO list must be one of the following:
  - An integer in the range of 0 to 255
  - A binary
  - An IO list
- IO lists are useful when you need to incrementally build a stream of bytes.
- A _macro_ consists of Elixir code that can change the semantics of the input code. A macro is always called at compile time; it receives the parsed representation of the input Elixir code, and it has the opportunity to return an alternative version of that code.
- If multiple modules are defined in a single source file, the compiler will produce multiple .beam files that correspond to those modules.
- When you start BEAM with Elixir tools (such as `iex`), some code paths are predefined for you. You can add additional code paths by providing the `-pa` switch.
- To dynamically call functions at runtime we can use `Kernel.apply/3` `apply(IO, :puts, ["Dynamic function call."])`
- `Kernel.apply/3` can be useful when you need to make a runtime decision about which function to call.
- If you don't want a BEAM instance to terminate, you can provide the `--no-halt` parameter.
- To create a new mix project, you can call `mix new project_name`
- Many functions from Elixir and Erlang return either {:ok, result} or {:error, reason}.
- When matching a map, the left-side pattern doesn’t need to contain all the keys from the right-side term.
- `rest::binary` states that you expect an arbitrary-sized binary
- If an error is raised from inside the guard, it won’t be propagated, and the guard expression will return false
- If an error is raised from inside the guard, it won’t be propagated, and the guard expression will return false
- Anonymous functions (lambdas) may also consist of multiple clauses.
- If the condition isn’t met and the else clause isn’t specified, the return value is the atom `nil`
- `with` special form, which can be very useful when you need to chain a couple of expressions and return the error of the first expression that fails.
- The `with` special form allows you to use pattern matching to chain multiple expressions, verify that the result of each conforms to the desired pattern, and return the first unexpected result.
- The benefit of `with` is that it returns the first term that fails to be matched against the corresponding pattern
- you can turn an operator into a lambda by calling &+/2, &\*/2, and so on. `Enum.reduce([1,2,3], 0, &+/2)`
- A _stream_ is a lazy enumerable, which means it produces the actual result on demand.
- `File.stream!/1` function, which takes the path of a file and returns a stream of its lines
- The `Map.update/4` function receives a map, a key, an initial value, and an _updater lambda_. If no value exists for the given key, the initial value is used. Otherwise, the updater lambda is called
- Each module can define only one struct, which can then be used to create new instances and pattern-match on them
- You can't call the `Enum` function on a struct.
- A struct pattern can't match a plain map, but a plain map pattern can match a struct.
- It’s important to be aware that data in Elixir is always transparent. Clients can read any information from your structs (and any other data type), and there’s no easy way of preventing that
- To see the pure data structre we can provide a special option to the `inspect` function. `IO.puts(inspect(mapset, structs: false))`
- This works because `IO.inspect/1` prints the data structure and then returns that same data structure unchanged.
- The original structure can be modified elegantly using the `Kernel.put_in/2` macro. `put_in(todo_list[3].title, "Theater")`
- Elixir provides similar alternatives for data retrieval and updates in the form of the `get_in/2`, `update_in/2`, and `get_and_update_in/2` macros. The fact that these are macros means the path you provide is evaluated at compile time and can’t be built dynamically.
- The code in Enum.each/2 is generic and relies on a contract. This contract, called a protocol, must be implemented for each data type you wish to use with Enum functions.
- A _protocol_ is a module in which you declare functions w/o implementing them
- You can send anything that implements `String.chars` to `IO.puts/1`
- You start the implementation by calling the `defimpl` macro. Then you specify which protocol to implement and the corresponding data type
- notice that the protocol implementation doesn’t need to be part of any module. This has powerful consequences: you can implement a protocol for a type even if you can’t modify the type’s source code
- If you want to control how your structure is printed in the debug output (via the `inspect` function), you can implement the _Inspect_ protocol.
- Arguably the most important protocol is `Enumerable`. By implementing it, you can make your data structure _enumerable_
- you can define your own protocols and implement them for any available data structure (your own or someone else’s). See the `Kernel.defprotocol/2` documentation for more information.
- When it’s passed to another process, the data is deep-copied, because two processes can’t share any memory.
- If a message doesn't match any of the provided clauses, it's put back into the process mailbox, and the next message is processed.
- A _server process_ is an informal name for a process that runs for a long time (or forever) and can handle various requests (messages)
- Waiting for a message puts the process in a suspended state and doesn't waste CPU cycles.
- It's perfectly normal to have different functions from the same module running in different processes.
- It's important to realize that a server process is internally sequential.
- Assuming that db queries can be run independently, you can start a pool of server processes, and then for each query somehow choose one of the processes from the pool and have the process run the query. If the pool is large enough and you divide the work uniformly across each worker in the pool, you'll parallelize the total work as much as possible.
- `:rand.uniform/1` function takes a positive integer and returns a random number in the range _1..N_ (inclusive)
- A typical stateful server technique is that you wait for a message and then, based on its contents, compute the new state. Finally, you loop recursively with the new state, effectively changing the state. The next received message operates on the new state.
- Registering a process can be done with `Process.register(pid, name)` where a name must be an atom.
- Although multiple processes may run in parallel, a single process is always sequential - it either runs some code or waits for a message.
- If a process doesn't handle some messages at all those messages will remain in the process mailbox forever, taking up memory space for no reason.
- For each server process, you should introduce a _match-all_ receive clause that deals with unexpected kinds of messages. Typically, you'll log that a process has received the unknown message, and do nothing else about it.
- Having many processes frequently send big messages may affect system performance.
- Less obviously, closing on a variable from a spawn also results in deep-copying the
  closed variable:
- A special case where deep-copying doesn’t take place involves binaries (including strings) that are larger than 64 bytes. These are maintained on a special shared binary heap, and sending them doesn’t result in a deep copy.
- `iex --erl "put Erlang emulator flags here"`
- There are some special cases when a process will implicitly yield execution to the scheduler before its execution time is up. The most notable situation is when using `receive`. Another example is a call to the `Process.sleep/1` function.
- By default, BEAM fires up 10 async threads, but you can change this via the `+A n` Erlang flag.
- Additionally, if your OS supports it, you can rely on a kernel poll such as _epoll_ or _kqueue_, which takes advantage of the OS kernel for nonblocking I/O. You can request the use of a kernel poll by providing the `+K true` Erlang flag when you start the BEAM.
- We use the term _call_ for synchronous requests. For asynchronous requests, we’ll use the term cast. This is the naming convention used in OTP, so it’s good to adopt it.
- The Erlang standard library includes the following OTP behaviours:
  - `gen_server` — Generic implementation of a stateful server process
  - `supervisor` — Provides error handling and recovery in concurrent systems
  - `application` — Generic implementation of components and libraries
  - `gen_event` — Provides event-handling support
  - `gen_statem` — Runs a finite state machine in a stateful server process
- `KeyValueStore.__info__(:functions)` : to see functions exported in a module after using `use GenServer`
- `:timer.send_interval/2` function periodically sends a message to the caller process.
- The `@impl GenServer` tells the compiler that the function about to be defined is a callback function for the `GenServer` behaviour.
- In `init/1`, you can decide against starting the server. In this case, you can either return `{:stop, reason}` or `:ignore`. In both cases, the server won't process with the loop, and will terminate.
- You should opt for `{:stop, reason}` when you can't proceed further due to some error.
- `:ignore` should be used when stopping the server is the normal course of action.
- Returning `{:stop, reason, new_state}` from `handle_*` callbacks causes `GenServer` to stop the server process.
- `:erlang_system_info(:process_count)` to count the number of currently running processes
- To encode an arbitrary Elixir/Erlang term, you use the `:erlang.term_to_binary/1` function, which accepts an Erlang term and returns an encoded byte sequence. The result can be stored on disk, retrieved at a later point, and decoded to an Erlang term with the inverse function `:erlang.binary_to_term/1`
- You should be careful about possibly long-running `init/1` callbacks.
- BEAM distinguishes three types of runtime errors: _errors_, _exits_, and _throws_
- An exit signal contains the pid of the crashed process and exit reason.
- In the case of a normal termination (when the spawned fn. has finished), the exit reason is the atom `:normal`
- Links are bi-directional in nature whereas monitors are unidirectional.
- The supervisor relies on the _maximum_ restart frequency, which defines how many restarts are allowed in a given time period.
- By default, the maximum restart frequency is three restarts in five seconds.
- We can change these parameters by passing `:max_restarts` and `max_seconds` options to `Supervisor.start_link/2`
- This is a property of supervisors. You can't keep a worker's pid for a long time because that process might be restarted, and its successor will have a different pid.
- Elixir's std lib. includes the implementation of a process registry in the `Registry` module.
- A _via tuple_ is a mechanism that allows you to use an arbitrary third-party registry to register OTP-compliant processes, such as `GenServer` and supervisor.
- Processes that shouldn't crash are informally called a system's _error kernel_ - processes that are critical for the entire system to work and whose state can't be restored in a simple and consistent way. Always have a recovery plan for the crash of a critical process.
- TIP : If you can afford to, it's better to start clean and terminate all other dependent processes because if error is caused by state that's somehow invalid which is persisted, then it can never restart successfully.
- The crash of a single task takes down all other tasks as well as the starter process.
- If a `GenServer`-powered module implements only `init/1`, `handle_cast/2`, and `handle_call/3`, it can be replaced with an `Agent`.
- `:ets.insert/2` perform key/value writes
- `:ets.delete/2` delete all rows associated with a given key
- `:ets.update_element/3` modify a row
- `:ets.update_counter/4` atomically update an integer in a row
- `:ets.tab2list/1` : converts the table to a list (less efficient)
- `:ets.first/1` and `:ets.next/2` make it possible to traverse the table iteratively.
- `:ets.match_object/2` accepts a match pattern and retrieves the rows. Performs filtering in ETS memory space, thus more efficient
- `:ets.match_delete/2` can be used to delete multiple objects with a single statement
- `:ets.select/2` can select rows based on match specifications.
- To perform complex load management : `gen_stage`

### Others

- `tuple_size` returns size of tuple
- `elem` allows you to use a zero based index to retrieve the values in a tuple.
- `put_elem(tuple, index, elem) e.g put_elem(pres_num_26, 0, "Franklin")`
- **GenServer** lets us implement some functions (called callbacks) for customizing its details:
  - `init/1` acts as a constructor, and is called when the server is started. The expected result is a tuple `{:ok, state}` that contains the initial state of the server.
  - `handle_call/3` is the callback that is called when a mesage arrives to the server. This is a synchronous function, and the result is a tuple `{:reply, response, new_state}` that contains the response to send to the caller, and the new state to apply to the server.
  - `handle_cast/2` is the asynchronous callback, which means that it doesn't have to respond to the caller directly. The expected result is a tuple `{:noreply, new_state}` that contains the new state.
  - `handle_info/2` receives all the messages that are not captured by other callbacks
  - `terminate/2` is called when server is about to exit, and is useful for cleanup
- By default, `Task.await` has a built-in timeout of 5 seconds. To override : `Task.await(task, 7000)`
- to be able to check if a long-running task has finished `Task.yield` comes in really hand.
- Use a **Task** if you want to perform a **one-off computation or query** asynchronously.
- Use an **Agent** if you just need a **simple process to hold state**
- Use a **GenServer** if you need a long-running server process that **stores state and performs work concurrently**
- Use a dedicated **GenServer** process if you need to **serialize access to a shared resource or service** used by multiple concurrent processes.
- Use a **GenServer** process if you need to **schedule background work to be performed on a periodic interval**
- `iex --sname node1@localhost` to start a node with name `node1`
- `node()` to obtain the name of the the started node
- `Node.connect(:node1@localhost)` connects to another node
- `Node.list/0` : returns a list of all nodes connected to the current one
- `Node.list([:this, :visible])` to get a list of all nodes in a cluster, including the current one.
- It is possible to register and receive notifications when a node is disconnected, using the `Node.monitor/1` function
- You can monitor all node connections and disconnections with the help of `:net_kernel.monitor_nodes`
- `Node.spawn/2` receives a node name and a lambda. This function then spawns a new process on the target node and runs lambda in that process.
- All standard I/O calls are forwarded to the _group leader_ - a process that's in charge of performing the actual i/p or o/p.
- It's possible to reference a locally registered process on another node by using `{some_alias, some_node}`. You can also use the `{some_alias, some_node}` form when making `GenServer`-powered requests.
- `GenServer.abcast/3` and `GenServer.multi_call/4`, that let you issue a request to all locally registered processes on given nodes.
- The simplest way to do cluster-wide discovery is to use the `:global` module, which provides a global name registration facility.
- `:global.register_name({:todo_list, "bob"}, self())`
- `:global.whereis_name({:todo_list, "bob"})`
- `:net_kernel` and `:net_adm`
- `:rpc` to obtain the result of a remote function call or invoke a function on multiple nodes and collect all the results. `:rpc.call(:node2@localhost), Kernel, :abs, [-1]`
- `:global_trans/2` takes the lock, runs the provided lambda, and finally released the lock.
- [Syn](github.com/ostinelli/syn) or [Swarm](github.com/bitwalker/swarm)
- You should set up a load balancer to serve as a single access point for all clients.
- You need a scheme for introducing new nodes to the running cluster. When a new node is introduced, it should first synchronize the db with one of the already-connected nodes
- Database replication is fragile. You need some kind of 2-phase commit strategy.
- You need to handle network partitions.
- `Node.monitor/2` to monitor a particular node
- Detecting a partition amounts to listening for `:nodedown` messages.
- In order to connect two nodes, they must agree on a magical cookie which is generated when BEAM starts and is different on machines. To connect two nodes on different machines you need to somehow make all nodes use the same cookie.
- `Node.get_cookie` can be used to see current cooking
- `Node.set_cookie(:some_cookie)`
- `iex --sname node1@localhost --cookie another_cookie`
- `iex --erl '-kernel inet_dist_listen_min 10000' --erl '-kernel inet_dist_listen_max 10100' --sname node1@localhost`
- `:net_adm.names()` can manually inspect the ports of all nodes on the host machine
- `epmd -names` from the OS command line
- If you are behind a firewall, you need to open port 4369 (EPMD) and the range of ports on which your node will listen.
- `elixir -S mix run --no-halt`
- `elixir --erl "-detached" --sname todo_system@localhost -S mix run --no-halt` runs in the background
- `iex --sname debugger@localhost --remsh todo_system@localhost --hidden` attaches to the running system and can be used for debugging
- An OTP _release_ is a standalone, compiled, runnable system that consists of the mini- mum set of OTP applications needed by the system.

- Which part is faciliating that messages from queues are picked in a round-robin or other manner? Is there a check to see if a node is overloaded or not to not send requests to it
- What is the maximum performance we can get from one node?

### GenServer callbacks and their expected return values

| Callback                               | Expected return values               |
| -------------------------------------- | ------------------------------------ |
| `init(args)`                           | `{:ok, state}`                       |
|                                        | `{:ok, state, timeout}`              |
|                                        | `:ignore`                            |
|                                        | `{:stop, reason}`                    |
| `handle_call(msg, {from, ref}, state)` | `{:reply, reply, state}`             |
|                                        | `{:reply, reply, state, timeout}`    |
|                                        | `{:reply, reply, state, :hibernate}` |
|                                        | `{:noreply, state}`                  |
|                                        | `{:noreply, state, timeout}`         |
|                                        | `{:noreply, state, hibernate}`       |
|                                        | `{:stop, reason, reply, state}`      |
|                                        | `{:stop, reason, state}`             |
| `handle_cast(msg, state)`              | `{:noreply, state}`                  |
|                                        | `{:noreply, state, timeout}`         |
|                                        | `{:noreply, state, :hibernate}`      |
|                                        | `{:stop, reason, state}`             |
| `handle_info(msg, state)`              | `{:noreply, state}`                  |
|                                        | `{:noreply, state, timeout}`         |
|                                        | `{:stop, reason, state}`             |
| `terminate(reason, state)`             | `:ok`                                |
| `code_change(old_vsn, state, extra)`   | `{:ok, new_state}`                   |
|                                        | `{:error, reason}`                   |

### Summary of the relationships b/w client API, GenServer, and callback functions

- `state` is shortened to `st`, and `from` is shortened to `fr`, and `pid` is `p`

| Metex.Worker client API | GenServer                  | Metex.Worker callback                   |
| ----------------------- | -------------------------- | --------------------------------------- |
| `MW.start_link(:ok)`    | `GS.start_link`            | `MW.init(:ok)`                          |
| `MW.get_temp(p, "NY")`  | `GS.call(p, {:loc, "NY"})` | `MW.handle_call({:loc, "NY"}, fr, st)`  |
| `MW.reset(p)`           | `GS.cast(p, :reset)`       | `MW.handle_cast(:reset, st)`            |
| `MW.stop(p)`            | `GS.cast(p, :stop)`        | `MW.handle_cast(:stop, st)`             |
|                         |                            | If this returns `{:stop, :normal, st}`  |
|                         |                            | then `MW.terminate(:normal, st)` called |

### A summary of the `Supervisor` API that you'll implement

| API                                          | Description                                                             |
| -------------------------------------------- | ----------------------------------------------------------------------- |
| `start_link(child_spec_list)`                | Given a list of child specifications (possibly empty) start the         |
|                                              | supervisor process and corresponding children                           |
| `start_child(supervisor, child_spec)`        | Given a supervisor pid and a child specification, start                 |
|                                              | the child process and link it to the supervisor                         |
| `terminate_child(supervisor, pid)`           | Given a supervisor pid and a child pid, terminate the child             |
| `restart_child(supervisor, pid, child_spec)` | Given a supervisor pid, a child pid, and a child specification, restart |
|                                              | the child process and initialize it with the child specification        |
| `count_children(supervisor)`                 | Given the supervisor pid, return the no. of child processes             |
| `which_children(supervisor)`                 | Given the supervisor pid, return the state of the supervisor            |

### TIL

- `Repo.one(from x in MyApp.Model, order_by: [desc: x.id], limit: 1)` : get the last record with Ecto
-

The amazing thing about Erlang and the BEAM is it's depth of features. To the OP the Behaviour/Interface of Erlang is their biggest take away. For me I believe it is how you require far far less development resources to build complex systems than you would require in any other language (provided comparable experience in both stacks). And for many the lightweight processes and programming model.
OTP itself has so much in it. We've been working on compiling Elixir to run on iOS devices. Not only can we do that through the release process but through using the ei library provided in Erlang we can compile a Node in C that will interface with any other Erlang node over a typical distributed network as you would for Erlang, Elixir, Gleam, etc... furthermore there is a rpc library in Erlang where from C we can make function calls and interface with our Elixir application. Yes, the encoding/decoding has an overhead and FFI would be faster but we're still way within our latency budget and we got this stood up in a few days without even have heard of it before.