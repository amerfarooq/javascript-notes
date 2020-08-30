1- The JavaScript engine sets up a memory space for variables and function before executing the code line-by-line. Functions are copied as-is to this space whereas variables are declared as undefined. This is known as hoisting. 

```javascript
function test(first, second) {
	log(first)
	log(second)
}
test()
```

2- undefined means that something has not been set. So don't assign variables the value undefined as it makes debugging difficult because in that case, you don't know whether you (the programmer) set the value as undefined or whether it was set by the JavaScript engine. Hence it's good to let undefined mean 'I have never set this value'. Use null instead.

3- JavaScript is synchronous.

4- Each function has a reference to its outer lexical environment i.e. the environment that encloses the functions body. So if a function cannot find a variable etc. inside its body, it goes to its outer environment to locate it there and so on. The variable is NOT looked up according to the current call stack.

```javascript
app.js
	
function a() {
  console.log(var);
}

function b() {
  var = 2;
  a();
}

var = 5;
a();
```

The following code logs the value 5 to the console. It does not matter that a() was called inside `b()`. What matters is that the outer environment of `a()` is the global execution context. So when `a() `does not find `var` inside its body, it DOES NOT look inside `b()` just because it was invoked there, but instead
looks for `var` inside the global execution context because that is its outer lexical environment.

P.S Lexical environment means where the code is physically placed inside the file. In app.js, `a()` is declared on a global level so its outer lexical environment is the global execution context. If it was declared inside` b()`, then `b()` would be its outer environment and the following code:

```javascript
app.js
	
function b() {

  function a() {
    console.log(var);
  }	
  var = 2;
  b();
}
var = 5;
a();
```

Will log the value 2 because now when `a()` does not find `var`, it looks inside `b()` because `b()` is in its outer lexical environment.

5-  JavaScript handles asynchronous callbacks in a manner similar to NodeJS in that an Event Queue is maintained and when the current call stack is finished, the associated handlers corresponding to completed events are added to the call stack and run.

6- Objects in JavaScript are collections of name-value pairs.

7- Primitive types contain only single values i.e. they are not objects.

8- We can use undefined, null and "" to check for the existence of something.

9- || (OR) operator returns the first value that coerces to true. We can use this to set default values for our functions like so:
	

```javascript
function (name) {
  name = name || 'Amer'
}
```

`name` is undefined by default so if no name is passed when the function is invoked, then undefined coerces to false but 'Amer' will coerce to true so 'Amer' will be returned and name will be set to it. If some name is provided, then its will coerece to true so the passed name is set as the value instead of the default 'Amer'.

10- `JSON.stringify` -> Convert to JSON   | `JSON.parse` -> Convert from JSON

11- A function is an object. It's name and code are its properties. This also means you can add new properties into functions such as variables, objects and even other functions.

12- Object literals:

```javascript
var new = {
	name: 'amer',
	age: 23
	grades {
		gpa: 3.9,
		letter: A
	}
}
```

13- Expression: A unit of code that results in a value.

14- Function statement: `function a() { console.log('Hello'); }`
       Function expression: `var b = function() { console.log('Hello'); }`

Function expressions are not hoisted because when the engine sees the variable, its places it in memory with a value of undefined. Hence, we cannot invoke function expressions before their declaration like we can with functions.

15- When using operators, even the '==' comparison operator, JavaScript coerces values to execute the operation so even though '1' + 2 doesn't make much sense, we get '12' which is a concatenated string because 2 (the Number) is coerced into a string.

16- Always use the === a.k.a the strict comparison operator for performing comparisons.

17- All primitive types are by value and all objects are by reference.

18- 'this' normally points to the global object (Window). Inside a method of an object, 'this' will reference the object itself. However, there is a bug in that if we create a new function inside of a method of an object, the 'this' keyword in that new function will point to the global object instead of the object it is defined in. So in this case:

```javascript
var a = function() {

	log: function() {
		console.log(this);		// 'this' references the 'a' object
		
		var new = function() {
			console.log(this);	// 'this' references the global object
		}
	}
}
```

A pattern to avoid this is to create a reference to the current object and use it inside the nested function instead of directly referencing 'this':

```javascript
var a = function() {

	log: function() {
		var self = this;	// Create reference to object 'a'
		
		var new = function() {
			self.whatever();
		}
	}
}
```

19- JavaScript arrays can hold different types of values e.g. `var newArray = [true, 1, 'yo', {name: Amer}]`

20- There is a special keyword inside every function called 'arguments' which is an array containing all the parameters passed to that function.

21- JavaScript automatically adds semicolons after a return if it doesn't see one. So this:
	
```javascript
function get() {
  return 
  {
    name: 'Amer'
  }
}
get()
	
// Doesn't return anything because the JavaScript engine automatically adds a semicolon after return. So it becomes:
	
function get() {	
  return; 	// Semicolon inserted
  {
    name: 'Amer'
  }
}
get()
	
// Nothing is returned. We need to amend it like so to get the desired result:
	
function get() {
  return {
    name: 'Amer'
  }
}
get()
```

22- Immediately Invoked Function Expression (IIFE - pronounced as iffy):

```javascript
var greet = function(name) {
  return 'Hello' + name;
}('Amer);
	
```
This immediately invokes the function expression and sets the variable greet to 'Hello Amer';	

```javascript
(function(name) {
return 'Hello' + name;
}('Amer));
```

Anything contained within `()` is considered an expression. The above expression contains an IIFE and will evaluate to 'Hello Amer'.

23- In the case of functions nested within functions, when the parent function finishes executing, its child functions still contain a reference to its variables. This is known as a closure. For the following code:

```javascript
function build() {
  var arr = [];

  for (var i = 0; i < 3; i++) {
    arr.push( function() {
      console.log(i);
    })
  }
  return arr;
}

fs = build();

fs[0]();
fs[1]();
fs[2]();
```

This will log the value '3' to the console for every invocation. This is because when `build()` is invoked, the variable `i` becomes equal to 3 at the end of the for-loop. Now when the functions in array `fs` are invoked, they don't have any variable `i` in their code so they look to their outer lexical environment i.e. the function `build().` And since in `build()` the variable `i` is equal to 3, they all log the same value. So even though the execution context of `build()` is removed when it is invoked, the functions nested inside it still have a reference to its variables and in this case, the value of the variable is the same for all of them.

The following code will produce results as we expect them:

```javascript
function build() {
	var arr = [];
	
	for (var i = 0; i < 3; i++) {
		arr.push(
			function(j) {
				return function() {
					console.log(j);
				}
			}(i))	
		})
	}
	return arr;
}
```

Here, we are using an IIFE to return a function that logs the value. This will create 3 separate execution contexts where the value of `j` will be different. So when `console.log` tries to log `j` and doesn't find it in its code, it will look inside its outer lexical environment, which is the IIFE that has the value
of `i`  passed to it. However, this time the outer environment for all the functions will be different so we will get our exprected results i.e. 0, 1 and 2.

Another example:

```javascript
function sayHi() {
  var greeting = 'Hello';

  setTimeout( function() {
    console.log(greeting);
  }, 3000);
}

sayHi();
```

This will log the greeting, even though when the function inside `setTimeout()` runs, it doesn't have any variable called 'greeting'. However, it does have a reference to the variables of its outer environment i.e. that of `sayHi()` and hence can access the value of 'greeting'.

Reference: JavaScript: Understanding the weird parts - Lecture 47, 49

24- `bind()` creates a copy of the function and changes the reference of 'this' to whatever we pass it.

```javascript
var person = {
	first: 'Amer',
	
	getName: function() {
		console.log(this.first);	// this references the person object. Look at point 18.
	}
}

var log = function() {
	this.getName()
}

log(); 	// Will return an error since inside log, 'this' references the global object which does not contain a getName() function.

var log2 = log.bind(person); 	 // Here we have created a copy of the log function where 'this' references the person object. So now, 'Amer' is logged to the console.
log2()

log.call(person); 				 // We directly invoke log and the passed person object is reffered to by 'this' inside it. 
log.call(person, arg1, arg2); 	 // Can also provide the arguments for log()

log.apply(person, [arg1, arg2])	 // Exactly same as call(). Only need to pass the arguments (if any) as a list.
```


​	
​	
​	
