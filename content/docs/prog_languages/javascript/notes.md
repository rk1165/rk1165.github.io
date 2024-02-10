# Browser JS

- In fact document.head and document.body are the only tags that we can access directly. All other tags must be accessed using querySelector
- <button>Click Me </button>
- <input type="text" value="changeme" size="40"> it also has a name attribute.
- type can be text, ,password, checkbox, radio, color,
- <input type="range" min=0 max=255 value=125> displays a slider
- <select>
    <option>High </option>
    <option>Medium</option>
  </select>
- Another event type that is commonly used with input is the onchange event.
- In fact there are three events you can experiment with for keyboard keys:
  - onkeydown – fires when the key is pressed
  - onkeypress – fires after onkeydown but not for control, shift, alt
  - onkeyup – fires when they key goes up
- have the color change as the bar moves! Change the event from onchange to onmousemove to see the results.
- **Creating an Element** The `document.createElement` function can be used to create any HTML element. You can create an h1, a table, a paragraph, an input, even a button. Once created, the new element is not yet part of our document tree. That is why we need to assign this newly created element a name
- **Creating Text** We also used the function `document.createTextNode` to create the text we are going to put in the table.
- **Adding to the Tree** Now that we have our new pieces for our page we need to put things back together. the `appendChild` function allows us to add to the tree by adding children one at a time
- We can get the elements from the list and put them in an array using the `querySelectorAll` funciton.
- If that user input needs to go back to the web server, then we need to enclose our input elements, and a submit button inside a `form`.
- method: this tells the browser which http method to use when submitting the form back to the server. The options are `get` or `post`.
- action: This tells the browser the URL to use when submitting the form
- The || operator, for example, will return the value to its left when that can be converted to true and will return the value on its right otherwise
- This functionality allows the || operator to be used as a way to fall back on a default value. If you give it an expression that might produce an empty value on the left, the value on the right will be used as a replacement in that case.
- The && operator works similarly, but the other way around. When the value to its left is something that converts to false, it returns that value, and otherwise it returns the value on its right
- A fragment of code that produces a value is called an expression. Every value that is written literally (such as 22 or "psychoanalysis") is an expression
- You can ask the user an OK/Cancel question using confirm. This returns a Boolean: true if the user clicks OK and false if the user clicks Cancel.
- The `prompt` function can be used to ask an “open” question. The first argument is the question, the second one is the text that the user starts with. A line of text can be typed into the dialog window, and the function will return this text as a string.
- The function Number converts a value to a number. We need that conversion because the result of prompt is a string value, and we want a number. There are similar functions called String and Boolean that convert values to those types.
- The `isNaN` function is a standard JavaScript function that returns true only if the argument it is given is `NaN`. The Number function happens to return `NaN` when you give it a string that doesn’t represent a valid number
- This “localness” of variables applies only to the parameters and to variables declared with the var keyword inside the function body. Variables declared outside of any function are called global, because they are visible throughout the program. It is possible to access such variables from inside a function, as long as you haven’t declared a local variable with the same name.
- In short, each local scope can also see all the local scopes that contain it
- If you pass too many, the extra ones are ignored. If you pass too few, the missing parameters simply get assigned the value undefined
- being able to reference a specific instance of local variables in an enclosing function—is called closure. A function that “closes over” some local variables is called a closure. This behavior not only frees you from having to worry about lifetimes of variables but also allows for some creative use of function values.
- when you read return function(...) {...}, think of it as returning a handle to a piece of computation, frozen for later use.
- value.x and value[x] access a property on value—but not necessarily the same property. The difference is in how x is interpreted. When using a dot, the part after the dot must be a valid variable name, and it directly names the property. When using square brackets, the expression between the brackets is evaluated to get the property name. Whereas value.x fetches the property of value named “x”, value[x] tries to evaluate the expression x and uses the result as the property name.
- Values of the type object are arbitrary collections of properties, and we can add or remove these properties as we please
- One way to create an object is by using a curly brace notation.
- Inside the curly braces, we can give a list of properties separated by commas. Each property is written as a name, followed by a colon, followed by an expression that provides a value for the property
- Properties whose names are not valid variable names or valid numbers have to be quoted.
- The delete operator cuts off a tentacle from such an octopus. It is a unary operator that, when applied to a property access expression, will remove the named property from the object.
- The binary in operator, when applied to a string and an object, returns a Boolean value that indicates whether that object has that property
- We can represent a two-by-two table in JavaScript with a four-element array ([76, 9, 4, 1]). We could also use other representations, such as an array containing two two-element arrays ([[76, 9], [4, 1]])
- The corresponding methods for adding and removing things at the start of an array are called `unshif`t and `shift`.
- The `indexOf` method has a sibling called `lastIndexOf`, which starts searching for the given element at the end of the array instead of the front.
- Both `indexOf` and `lastIndexOf` take an optional second argument that indicates where to start searching from.
- Another fundamental method is slice, which takes a start index and an end index and returns an array that has only the elements between those indices
- The `concat` method can be used to glue arrays together,
- The trim method removes whitespace (spaces, newlines, tabs, and similar characters) from the start and end of a string
- Accessing the individual characters in a string can be done with the `charAt` method but
- Whenever a function is called, a special variable named arguments is added to the environment in which the function body runs. This variable refers to an object that holds all of the arguments passed to the function.
- in JavaScript you are allowed to pass more (or fewer) arguments to a function than the number of parameters the function itself declares
- The arguments object has a length property that tells us the number of arguments that were really passed to the function. It also has a property for each argument, named 0, 1, 2, and so on.
- The previous example uses `Math.random`. This is a function that returns a new pseudorandom number between zero (inclusive) and one (exclusive) every time you call it.
- If we want a whole random number instead of a fractional one, we can use `Math.floor` (which rounds down to the nearest whole number) on the result of `Math.random`.
- The global scope, the space in which global variables live, can also be approached as an object in JavaScript. Each global variable is present as a property of this object. In browsers, the global scope object is stored in the window variable
- Sometimes it is useful to check if the property of a given object exists or not. We can use the `.hasOwnProperty(propname)` method of objects to determine if that object has the given property name. `.hasOwnProperty()` returns true or false if the property is found or not.
- To use `reduce` you pass in a callback whose arguments are an accumulator (in this case, `(previousVal`) and the current value `(currentVal)`.
- The accumulator is like a total that `reduce` keeps track of after each operation. The current value is just the next element in the array you're iterating through.
- `reduce` has an optional second argument which can be used to set the initial value of the accumulator. If no initial value is specified it will be the first array element and `currentVal` will start with the second array element.
- The `filter` method is used to iterate through an array and filter out elements where a given condition is not true.
- `filter` is passed a callback function which takes the current value (we've called that val) as an argument.
- Any array element for which the callback returns true will be kept and elements that return false will be filtered out.
- You can use the method sort to easily `sort` the values in an array alphabetically or numerically.
- Unlike the previous array methods we have been looking at, `sort` actually alters the array in place. However, it also returns this sorted array.
- `sort` can be passed a compare function as a callback. The compare function should return a negative number if `a` should be before `b`, a positive number if a should be after b, or 0 if they are equal.
- If no compare (callback) function is passed in, it will convert the values to strings and sort alphabetically.
- You can use the `reverse` method to reverse the elements of an array.
- `reverse` is another array method that alters the array in place, but it also returns the reversed array.
- `concat` can be used to merge the contents of two arrays into one.
- `concat` takes an array as an argument and returns a new array with the elements of this array concatenated onto the end.
- Functions that operate on other functions, either by taking them as arguments or by returning them, are called higher-order functions.
- `apply` method. You pass it an array (or array-like object) of arguments, and it will call the function with those arguments.
- JavaScript gives us functions, `JSON.stringify` and `JSON.parse`, that convert data to and from this format. The first takes a JavaScript value and returns a JSON-encoded string. The second takes such a string and converts it to the value it encodes.
- The parameters to the reduce function are, apart from the array, a combining function and a start value.
- The `bind` method, which all functions have, creates a new function that will call the original function but with some of the arguments already fixed.

### Random

- It only works in languages where promises/futures/tasks are a first-class citizen. Eg JavaScript.
- When caching the result of an expensive computation or a network call, don't actually cache the result, but cache the promise that awaits the result. Ie don't make a `Map<Key, Result>` but a `Map<Key, Promise<Result>>`
- Event loop's job is to look at the task queue and the stack. If the stack is empty it takes the first thing on the queue.

### Node and JS

- Using docker volume to store postgres data where it is backed up
- `docker run --name pg -p 5432:5432 -v /home/user/data/pg:/var/lib/postgresql/data postgres`
- we create a new folder pgbrandnew with pg contents
- `docker run --name pg -p 1234:5432 -v /home/user/data/pgbrandnew:/var/lib/postgresql/data postgres`
- To store/remove object/file locally using `creatingObjectURL` and `RevokeObjectURL`;
- `var` variables get attached to window. It is not garbage collected out of scope. Never use it. Most of the time we are fine with `const`. `let` is used in loop
- Promises are JavaScript construct that allow us to perform async calls w/o blocking the UX, UI or client.
- They are designed for operations that include lot of waiting like REST call, network call.
- Webhooks are custom callbacks URLs that an application can call to communicate with another application. A popular uses of webhooks are github and discord. Where discord creates a webhook and you can share that webhook for other applications to “hook” into it to post messages discord when something happened in the application. When I upload a video post a link to discord

```javascript
const whurl = "https://discordapp.com/api/webhooks/6...";
const msg = {
  content: "Hello! I'm a bot, this is fetch api",
};
fetch(whurl + "?wait=true", {
  method: "POST",
  headers: { "content-type": "application/json" },
  body: JSON.stringify(msg),
})
  .then((a) => a.json())
  .then(console.log);
```

- `r.json()`, `r.text()`, `r.blob()` : in `fetch`
- Server-Sent Events or SSE is when the server sends events to the client in a unidirectional manner.
- SSE Use cases : Live feed, Showing client progress, logging
- Failover is when a server goes down, a client automatically directs traffic to other server without knowing a failover has happened.
- **VRRP** : Virtual Router Redundancy Protocol : Multiple machines share the same VIP and they talk to each other sending heartbeat saying who is the leader
- IANA MAC : Two server who has the same VIP address has the same Virtual IANA MAC.
- Both gets the frame when client request, but only the master responds and the backup drops the frame.

```javascript
let ws = new WebSocket("ws://0.tcp.ngrok.io:18394");
ws.onmessage = console.log;
ws.send("hi! I'm a public babe");
```

- HTTP Request Smuggling
- Protocol Ossification