+++
title = 'JavaScript Notes'
date = 2024-02-11

+++

### Browser JS

- In fact document.head and document.body are the only tags that we can access directly. All other tags must be accessed using querySelector
- `type` can be text, password, checkbox, radio, color, range
- <input type="text" value="changeme" size="10" name="inp"> 
- <input type="range" min=0 max=255 value=150>
- <select>
    <option>High </option>
    <option>Medium</option>
  </select>
- Another event type that is commonly used with input is the `onchange` event.
- In fact there are three events you can experiment with for keyboard keys:
  - `onkeydown` – fires when the key is pressed
  - `onkeypress` – fires after onkeydown but not for control, shift, alt
  - `onkeyup` – fires when they key goes up
- have the color change as the bar moves! Change the event from `onchange` to `onmousemove` to see the results.
- **Creating an Element** The `document.createElement` function can be used to create any HTML element. You can create an h1, a table, a paragraph, an input, even a button. Once created, the new element is not yet part of our document tree. That is why we need to assign this newly created element a name
- **Creating Text** We also used the function `document.createTextNode` to create the text we are going to put in the table.
- **Adding to the Tree** Now that we have our new pieces for our page we need to put things back together. the `appendChild` function allows us to add to the tree by adding children one at a time
- We can get the elements from the list and put them in an array using the `querySelectorAll` funciton.
- The || operator, will return the value to its left when that can be converted to true and will return the value on its right otherwise
- This functionality allows the || operator to be used as a way to fall back on a default value. If you give it an expression that might produce an empty value on the left, the value on the right will be used as a replacement in that case.
- The && operator works similarly, but the other way around. When the value to its left is something that converts to false, it returns that value, and otherwise it returns the value on its right
- A fragment of code that produces a value is called an expression. Every value that is written literally (such as 22 or "psychoanalysis") is an expression
- You can ask the user an OK/Cancel question using confirm. This returns a Boolean: true if the user clicks OK and false if the user clicks Cancel.
- The `prompt` function can be used to ask an "open" question. The first argument is the question, the second one is the text that the user starts with. A line of text can be typed into the dialog window, and the function will return this text as a string.
- The function `Number` converts a value to a number. We need that conversion because the result of prompt is a string value, and we want a number. There are similar functions called String and Boolean that convert values to those types.
- The `isNaN` function is a standard JavaScript function that returns true only if the argument it is given is `NaN`. The Number function happens to return `NaN` when you give it a string that doesn’t represent a valid number
- This "localness" of variables applies only to the parameters and to variables declared with the `var` keyword inside the function body. Variables declared outside of any function are called global, because they are visible throughout the program. It is possible to access such variables from inside a function, as long as you haven’t declared a local variable with the same name.
- If you pass too many, the extra ones are ignored. If you pass too few, the missing parameters simply get assigned the value undefined
- `value.x` and `value[x]` access a property on `value` — but not necessarily the same property. The difference is in how `x` is interpreted. When using a dot, the part after the dot must be a valid variable name, and it directly names the property. When using square brackets, the expression between the brackets is evaluated to get the property name. Whereas `value.x` fetches the property of value named "x", `value[x]` tries to evaluate the expression x and uses the result as the property name.
- Properties whose names are not valid variable names or valid numbers have to be quoted.
- The `delete` operator cuts off a tentacle from such an octopus. It is a unary operator that, when applied to a property access expression, will remove the named property from the object.
- The binary `in` operator, when applied to a string and an object, returns a Boolean value that indicates whether that object has that property
- The corresponding methods for adding and removing things at the start of an array are called `unshift` and `shift`.
- The `indexOf` method has a sibling called `lastIndexOf`, which starts searching for the given element at the end of the array instead of the front.
- Both `indexOf` and `lastIndexOf` take an optional second argument that indicates where to start searching from.
- Another fundamental method is `slice`, which takes a start index and an end index and returns an array that has only the elements between those indices
- The `concat` method can be used to glue arrays together
- The `trim` method removes whitespace (spaces, newlines, tabs, and similar characters) from the start and end of a string
- Accessing the individual characters in a string can be done with the `charAt` method but
- Whenever a function is called, a special variable named arguments is added to the environment in which the function body runs. This variable refers to an object that holds all of the arguments passed to the function.
- in JavaScript you are allowed to pass more (or fewer) arguments to a function than the number of parameters the function itself declares
- The arguments object has a `length` property that tells us the number of arguments that were really passed to the function. It also has a property for each argument, named 0, 1, 2, and so on.
- `Math.random` returns a new pseudorandom number between zero (inclusive) and one (exclusive) every time you call it.
- If we want a whole random number instead of a fractional one, we can use `Math.floor` (which rounds down to the nearest whole number) on the result of `Math.random`.
- The global scope, the space in which global variables live, can also be approached as an object in JavaScript. Each global variable is present as a property of this object. In browsers, the global scope object is stored in the window variable
- Sometimes it is useful to check if the property of a given object exists or not. We can use the `.hasOwnProperty(propname)` method of objects to determine if that object has the given property name. `.hasOwnProperty()` returns true or false if the property is found or not.
- To use `reduce` you pass in a callback whose arguments are an accumulator (in this case, `(previousVal`) and the current value `(currentVal)`.
- The accumulator is like a total that `reduce` keeps track of after each operation. The current value is just the next element in the array you're iterating through.
- `reduce` has an optional second argument which can be used to set the initial value of the accumulator. If no initial value is specified it will be the first array element and `currentVal` will start with the second array element.
- `sort` actually alters the array in place. However, it also returns this sorted array.
- `sort` can be passed a compare function as a callback. The compare function should return a negative number if `a` should be before `b`, a positive number if a should be after b, or 0 if they are equal.
- If no compare (callback) function is passed in, it will convert the values to strings and sort alphabetically.
- `apply` method. You pass it an array (or array-like object) of arguments, and it will call the function with those arguments.
- JavaScript gives us functions, `JSON.stringify` and `JSON.parse`, that convert data to and from this format. The first takes a JavaScript value and returns a JSON-encoded string. The second takes such a string and converts it to the value it encodes.
- The `bind` method, which all functions have, creates a new function that will call the original function but with some of the arguments already fixed.

### Node and JS

- Using docker volume to store postgres data where it is backed up
- `docker run --name pg -p 5432:5432 -v /home/user/data/pg:/var/lib/postgresql/data postgres`
- we create a new folder pgbrandnew with pg contents
- `docker run --name pg -p 1234:5432 -v /home/user/data/pgbrandnew:/var/lib/postgresql/data postgres`
- To store/remove object/file locally using `creatingObjectURL` and `RevokeObjectURL`;
- `var` variables get attached to window. It is not garbage collected out of scope. Never use it. Most of the time we are fine with `const`. `let` is used in loop
- Promises are JavaScript construct that allow us to perform async calls w/o blocking the UX, UI or client.
- They are designed for operations that include lot of waiting like REST call, network call.
- Webhooks are custom callbacks URLs that an application can call to communicate with another application. A popular uses of webhooks are github and discord. Where discord creates a webhook and you can share that webhook for other applications to "hook" into it to post messages discord when something happened in the application. When I upload a video post a link to discord
