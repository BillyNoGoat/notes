---
export_on_save:
  html: true
---

### Execution context

Given the following code:
```javascript {cmd="node"}
b();
console.log(a);

var a = 'hello world';

function b(){
  console.log('Called b');
}
```

In most programming languages, this would not properly run as you cannot call a function before it has been created. In Javascript this is not the case.
As such, this returns the following output:

```
called b
Undefined
```

### Hoisting

If we were to remove the line `var a = 'hello world';` we would actually see an error `Uncaught ReferenceError: a is not defined`.

This is called "Hoisting".

The execution context is created in two phase. The first phase is the creation phase where the Global Object and `this` are both created. `this` is always created within an execution context.

During this phase, as the parses parses your code to set up what you've written, it sets up the memory space for the variables and functions. It's this step which is referred to as hoisting. The name indicates (And many explain this in this way) that the code is being hoisted to the top of the code to be executed first. In actual fact, hoisting just refers to this memory reservation in the creation phase.

During the creation phase, the parser will identify functions and where variables should exist. The *assignment* of values to variables is done during the execution phase only (when the code is being executed).

All variables start as `undefined` in javascript during the creation phase and functions are held in memory in their entirety.

This is why you want to avoid relying on hoisting. It is best to stick to traditional programming practices and creating things in the right order.

### Undefined

We've already seen there's a first phase of creating an execution context where there's a global object, `this`, a reference/link to the outer environment and hoisting where variables are initially set to undefined. But what does this mean?

Using the following code as reference:
```javascript {cmd="node"}
var a;
console.log(a);
```

This will return `undefined` when run.
As we saw before, if we remove the line `var a;` we will get an error instead saying `a is not defined`.

Being undefined and not defined are two different things in Javascript. When we see `undefined` in Javascript, it isn't simply a string `undefined` it's actually a special value that Javascript has that means the variable hasn't been set to any value but exists in memory. As such, `undefined` does not need to be used in quotations as it is it's own special value in Javascript.

When a variable isn't declared, it gives an error as it is saying there is no memory allocated for that variable.

Never set a value to `undefined`. It's perfectly valid Javascript but it's dangerous. It should ALWAYS mean that you never set the value.

### Execution context: Code Execution
As mentioned, there are two phases when running Javascript. First is the creation phase and second is the execution phase.

In the execution phase we already have all the things we had before but now during the execution phase, it runs your code line by line.

### Threading
#### Single threaded
Single threaded execution means you're only dealing with one line at a time. Javascript behaves in a single threaded manner and should be treated as single threaded even if under the hood (The client itself) it's being run multi threaded.

#### Synchronous
Synchronous means it's being run one line of code at a time. Javascript is run in single threaded synchronous manner.
This obviously is different when it comes to AJAX (Will discuss later) and node.js.

### Function Invocation and the execution stack
Invocation just means running/calling a function. When we say we invoke a function we are just referring to the calling the function like `myfunction()`.

Look at the following:

```javascript {cmd="node"}
function b(){

}

function a(){
  b();
}

a();
```

When this is run, it creates a global execution context and completes it's two phases as mentioned earlier but also each time we invoke a function, we are moving straight into invoked function and a new execution context is created for each function which means each function that gets invoked will go through both initial phases of creation and execution and will get it's own `this` and global object.

This is how function invocation in Javascript works and this is called the *Execution stack*.

You an imagine the execution stack like an actual stack where the function on top is the current function that is being run and is removed once the function has been completed.

For example, in that code, when function a() is called, a new execution context is created and "Stacked" on top of the global execution context. This then invokes another function before being completed which is `b()` and therefore the `a()` execution context is not removed from the stack but a new execution context is put on the top of the stack which is `b()`. Once all code in that context has been run, it is removed from the stack and continues running a from where it left off when it invoked the last function.

The lexical position of the code in the page does not matter in this regard. Javascript will only care about the order in which functions are being invoked.

Even if a function is invoking itself, it will create a new execution context to place on top.

### Variable Environment
A variable environment refers to where the variable lives and how it relates in memory.

When you think about "variable environment" think about "Where is the variable?"

If we look at the image below:

![img-paste-20180201212301684.png](img-paste-20180201212301684.png)

For each execution context, we can see that the variable myVar gets its own dedicated store in memory per execution context. They do not share the same section in memory.

We can also see that in function b() myVar is equal to undefined. This is due to **scope** which essentially defines where we can see the variable. Each variable we're looking at is defined in it's own execution context as it's all in it's own function. Even though myVar is created 3 times, they are 3 distinct times which do not see each other and will never interact.

### Scope chain
Look at the following code:
```javascript {cmd="node"}
function b(){
console.log(myVar);
}

function a(){
  var myVar = 2;
  b();
}

var myVar = 1;
a();
```

In this situation, we get `1` as an output to this.

The reason for this is due to the scope chain. When we do something with a variable, Javascript will do more than just look in it's current execution context. We need to remember that every execution context (Aside from global) has a reference to it's outer environment.

In the case of the above code, even though function b() is 1 step above function a() in terms of execution contexts, the outer execution context is the global execution context due to the fact that the variable has been created inside of the global execution context. How it gets invoked/what order does not matter in this situation.

In Javascript, when trying to use a variable inside of an execution context, if it cannot find one in it's current execution context, it will go to it's outer execution context to check if it exists there. It will keep going through outer environments until it either cannot find an outer context (If you're in global context) or it finds the variable you are looking for in an outer execution context.

This whole chain of outer environment references is called the **Scope chain**.

To show this, if we look below:
```javascript {cmd="node"}
function a(){

  function b(){
    console.log(myVar);
  }

  var myVar = 2;
  b();
}

var myVar = 1;
a();
```
Note that if we were to put `b();` at the bottom line after `a();` we would get an error saying `b();` was not defined. This is because `b();` does not lexically sit in the global execution context like a() does. In order to get to `a();`, we must be inside of the execution context that it lives in which in this case would be `a();`

This code will now return `2` as the first execution context from `b();` where myVar is being requested is `a();`'s execution context.

### Scope, ES6 and let
Scope is just where about in your code a variable is available and if it's truly the same variable or a new copy.

In ES6, we are allowed to use a new way of declaring variables using the keyword `let`.

let allows us to do something called block scoping. See the code below:
```javascript {cmd="node" output="none"}
if (a > b){
  let c = true;
}
```
The difference with using let over var is that although it is still being put into memory during the creation phase just like `var`, it cannot be used until the line of code is run during the execution phase that actually declared the variable.

Another thing to note is that it's declared inside a block. A block is just an area of code between curly braces such as an if statement or for loop. When that variable is made in that block, it is ONLY available in that block. This is true also inside for loops. If you have a loop which is going through a for loop over and over, you will actually get the same variable re-created over and over in memory. This allows for block scoping.

### Asynchronous Javascript
Asynchronous simply means "More than one at a time". Javascript in the browser is synchronous but there are events such as click events and data retrieval which are asynchronous.

##### How is asynchronous possible in a browser?

To understand this, we need to understand the Javascript engine itself. When we're talking about running Javascript, we aren't just talking about the Javascript engine itself. The Javascript engine is just part of a web browser which makes up a user experience on a webpage. There are other parts which make this experience for the user and Javascript has hooks into them. See the image below to see two other parts of a web browser that the Javascript engine can interact with via it's hooks whilst the Javascript engine itself runs synchronously.

![img-paste-20180201220706122.png](img-paste-20180201220706122.png)

##### How can we use asynchronous code?
Just as we saw with the execution stack, there is another list we call the **Event queue**. This is full of events/notifications of events that might be happening. When something happens that we might want to be notified of, it gets placed in an event queue. A click event for example. If we have a function that should respond to a click event or HTTP request, these requests are listened for and any time an event is fired it's put into the event queue stacked on one each other.

The event queue does not get processed until there is nothing left in the execution stack. Until Javascript is finished running all the code in the execution stack, the event queue will not be processed. One the stack is empty, the first event in the event queue will be run and will create new execution contexts for functions that handle that event and only when those newly created execution contexts in the stack are processed will it move onto the next event in the event queue and continue it's process.

We can understand the synchronous nature and how these stacks relate with the following code:

```javascript {cmd="node" output="none"}
// long running function
function waitThreeSeconds() {
    var ms = 3000 + new Date().getTime();
    while (new Date() < ms){}
    console.log('finished function');
}
˜
function clickHandler() {
    console.log('click event!');
}

// listen for the click event
document.addEventListener('click', clickHandler);


waitThreeSeconds();
console.log('finished execution');
```
The above code creates a timer when the Javascript gets loaded that waits 3 seconds before completing the function. As a result, due to being synchronous, this means that the code will block the execution of even an asynchronous action such as a click event until the execution stack is completely clear and the 3 second timer is done.

### Types
*Dynamic typing* is where we don't tell the engine what type of data a variable holds, instead it's figured out as it's running. A variable can hold different types of data in the same variable.

In other languages, *static typing* is used where you define what data type goes into a variable and data that isn't of the type originally declared cannot be stored in that variable.

### Primitive Types
There are 6 primitive types. A primitive type is a type of data that represents a single value. In other words, something that isn't an object (Collection of name pairs).

| Data Type | Description                                                                                                                                |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Undefined | Represents a lack of existence. We should not set this value ever as we should let the engine set this value.                              |
| Null      | Also represents a lack of existence. We should use this to set something to nothing instead of undefined.                                  |
| Boolean   | True or false                                                                                                                              |
| Number    | A floating point number meaning there is always some decimals attached to it. Unlike other languages, there's only floating point numbers. |
| String    | A sequence of characters                                                                                                                   |
| Symbol    | Used in ES6 and is being constructed and is not yet supported in all browsers. We will ignore this for now - Just know it exists.          |

### Operators
An operator is a special function that is syntactically (written) differently to a traditional function. Normally taking two parameters to return one result.

Operators are essentially functions that can be called in a very short hand for example if doing `5 + 5;` we can write in "Infix" notation meaning we put the operator in the middle of parameters. It's a very shorthand way of calling a predefined function Javascript has to add two numbers together.

#### Operator precedence and associativity
Operator precedence just means which operator function gets called first when there's more than one on the same executable code. The higher precedence wins and will be executed first.

Operator associativity is what order the operator will get called in. This is when an operator has the same precedence as each other. At this point, depending on the associativity of the operators the engine will decide to run left-right or right-left.

An example of where this is important: In Mathematics in general, there's a mathematical precedence associated with mathematical operators and therefore Javascript will need to ensure that a calculation such as `5 + 5 * 3` is executed in the correct order as it should be in Mathematics. This is where operator precedence comes in.

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence

Above is a link showing a table of all operators in Javascript and their precedence and Associativity. The `...` around the operators are just other code.

If we have two operators with the same precedence, Javascript will need to use associativity to understand in which way the code should be executed.

Take the following example:
```javascript {cmd="node"}
var a = 1, b = 2, c = 3;
a = b = c;
console.log(a);
console.log(b);
console.log(c);
```

If we check the output of this, we can see that 3 is returned all times. If we check the MDN linked above we can see that associativity for the `=` operator is right-left meaning we start from the right of the line and execute left.

As mentioned earlier, assigning operators generally take 2 parameters and return a value. This is also true in assignment. If we do `b = c` this runs a function (Called shorthand by the operator `=`) and returns the value that is being assigned.

##### Grouping
It is worth nothing that parenthesis `()` are the highest possible precedence meaning everything inside it will be run first before everything else. This is useful for ensuring your code is executed in the correct order.

### Coercion
Coercion is when you are converting a value from one type to another. This happens a lot in Javascript due to being dynamically typed.

If we look at the following code:
```javascript {cmd="node"}
var a = 1 + "2";
console.log(a);
```
We can see that this outputs as 12 thanks to Javascript's type conversion. This is because 1 was automatically converted to a string. This was chosen by the engine and not specified by the user. Javascript knows it's impossible to add a number and a string together so to allow this operation to take place, 1 will be automatically converted to a string.

### Comparison operators
#### Quirks
Examine the following code:
```javascript {cmd="node"}
console.log(1 < 2 < 3);
```
This returns true which is what we would expect as 3 is greater than 2 which is greater than 1. This is a true statement and should return true.

However, looking at the following code:
```javascript {cmd="node"}
console.log(3 < 2 < 1);
```
This should return false as 1 is not greater than 2 which is not greater than 3. But it returns true.
The reason for this is that using an operator `<` returns a boolean true or false. If we look at the precedence table, we can see that the associativity of the `<` operator reads left to right.
This means that after the first operation is completed we are running the following:

```javascript {cmd="node"}
console.log(false < 1);
```
Going from left to right (as we should do for this associativity) we can see this returns the boolean of false. Now due to Javascript's automatic type conversion, false is converted to a number which actually is converted to `0`. For reference, converting a boolean `true` will return `1`.

We can see this by running the following:
```javascript {cmd="node"}
console.log(Number(false));
console.log(Number(true));
```

This means our original execution now reads as if it is written:
```javascript {cmd="node"}
console.log(0 < 1);
```
Which we know is true and returns true. This is why this shows as true even though from our human perspective this seems abnormal.

#### Equality operator
The equality operator returns a boolean and will check the 2 parameters passed and ensure their values are the same.
Due to coercion we can run commands such as:
```javascript {cmd="node"}
console.log("5" == 5);
```
This will coerce the two parameters into the same type and perform the comparison.

Unfortunately there are lots of quirks with automatic type coercion, especially with the equality operator.

For example:
```javascript {cmd="node"}
console.log(Number(null));
```
When Null is coerced into a number, 0 is returned. However, running the comparison:
```javascript {cmd="node"}
console.log(null == 0);
```
This returns us **false** even though if null were coerced into a number it would have become 0 and the comparison operator would have returned **true**.

However, some operators would return true. For example:
```javascript {cmd="node"}
console.log(null < 1);
```
Here we see that null is being coerced with the `<` operator and therefore returns true.

This just shows how Javascript can be difficult to anticipate.

#### Strict equality
Strict equality will ensured that both values are the same type when comparing.
We should try to do comparisons against variables of the same type. We should use `===` 99% of the time. `==` should **only** be used when explicitly aware of what is does and when it is absolutely required.

For example:
```javascript {cmd="node"}
console.log("5" === "5");
console.log(5 === 5);
console.log(5 === "5");
```
There is an MDN article on sameness between variables including a table of how comparisons will return in Javascript:

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness

#### Existence and Booleans
Look at the following code:
```javascript {cmd="node"}
console.log(Boolean(undefined));
console.log(Boolean(null));
console.log(Boolean(""));
```
We can see that all of these absence of values return false. Here we can use automatic type conversion to our advantage to check for existence.

```javascript {cmd="node"}
var a;

if (a) {
  console.log('Something is there.');
}
```
We can see that this returns us nothing. This can be used to use check if something has an assigned value or not.

##### The caveat
The caveat to this is that `0` will be automatically type converted to false too. If we know that a value will never be a 0 then we will be fine to leave it as is. However, if we know a 0 could exist in that variable we can change our condition to have a strict equality operator and an `||` OR to check for 0.

#### Default value
Check the following code:
```javascript {cmd="node"}
function greet(name){
  console.log('Hello ' + name);
}

greet();
```

We can see that when even though we do not pass in a `name` parameter, we can see the code executes without issues. Javascript does not care if you give it all the correct parameters, it will attempt to execute anyway.

Here we can see that greet was called and the execution context for this function set up the memory space for variable `name` but did not assign a value. Since we did not re-assign this memory to a new value, it has stayed as `undefined` and is being coerced into becoming a string and getting concatenated onto the end of the string.

We can use a neat trick to set a default value. In ES6 there are other ways of setting default values but this default code will often be seen in legacy code so is important.

See the trick below:
```javascript {cmd="node"}
function greet(name){
  name = name || 'Billy';
  console.log('Hello ' + name);
}

greet();
```
To understand how this works we need to pay attention to the OR (`||`) operator.

If you pass the OR operator 2 values, it will attempt to coerce them to true or false and return the first true value.

We can also use this example to see how precedence works on operators. `||` has a higher precedence than the `=` operator so OR runs before assignment.

##### Be careful of 0 again
As mentioned earlier, 0 will be evaluated as false. Be aware of it.

```javascript {cmd="node"}
console.log(false == null);
```
