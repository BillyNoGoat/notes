##ES6 Let, Var and Const

In ES5, we only had one way to declare a variable which was `var` but since ES6 we now have the ability to use `let` and `const`.

In most programming languages, the following code would not run:

```javascript {cmd="node"}
for (var i = 0; i < 10; i++){
  console.log(i);
}
console.log('The end value of variable i is: ' + i);
```
The reason this would not normally run is that `i` in this case would normally be considered **out of scope** since it's inside of a for loop. In Javascript, the scope is only defined by our functions as with `var` we do not have the concept of `block scope` (Pretty much scope for anything within `{}`)

If we were to wrap this entire for loop inside of a function and call the function and THEN try to log the variable `i` it would simply error when attempting to access variable `i`:

```javascript {cmd="node"}
function counter(){
  for (var i = 0; i < 10; i++){
    console.log(i);
  }
}

counter();
console.log('The end value of variable i is: ' + i);
```
This is because at THIS point we have hit the limitations of the scope for the `var` keyword.

####**NOTE**
Now we need to keep in mind the concept of **hoisting**.
When the variable is declared in any function, it essentially moves (hoists) that variable to the top of that execution context (top of the function for example) and declares it above all your code.
If we do not explicitly tell Javascript to declare a variable in that scope, it will declare a global variable instead. See how the following code **doesn't** error since `i` is being declared in the global scope due to the lack of `var` keyword:

```javascript {cmd="node"}
function counter(){
  for (i = 0; i < 10; i++){
    console.log(i);
  }
}

counter();
console.log('The end value of variable i is: ' + i);
```

###let
Let introduces **block scope** where almost everything using `{}` to create a **block** will have this vairable scoped to only that block. For example, an if statement, while loop, fuction etc. If you **know** a vairable should **only** be used inside of a specific block, you should use `let` to ensure you do not have any collisons later down the line.

###Const
Const works just as `let` does except you cannot re-assign `const`.
Using `const` does NOT make things completely immutable. For example, when `const` refers to an object, you can change the value of object properties but you cannot re-assign the variable `const` itself to point to something else such as another object, string, number etc.

##Best practice
We should be using `const` whenever we have variables we know we do not need to re-assign and use `let` whenever we do need to re-assign variables.

This is because it is important to **minimize mutable state** meaning that we want to minimize the amount that can change in our code to ensure we have a stricter experience less prone to issues.
