### Generator functions
View the following code:
```javascript {cmd="node"}
var hello = function(name){
  return 'Hello ' + name;
}

console.log(hello('James'));
```
This is a simple function which returns a value to the console.

Now if we change this to a generator function, we can do that by adding a `*` after the function keyword. Now let's run the same script:

```javascript {cmd="node"}
var hello = function *(name){
  return 'Hello ' + name;
}

console.log(hello('James'));
```
Here, we are not returned a string, we are instead returned an empty object. When we thought we were calling the generator function, we weren't. We were actually creating the generator object.

To start the generator function, we need to call that with the `next` method:

```javascript {cmd="node"}
var hello = function *(name){
  return 'Hello ' + name;
}

var gen = hello('James');
console.log(gen.next());
```

Now if we run the script, we get back an object with two attributes. `value` and `done`. Generators can be paused and resumed. We can pause the execution of a function by using the keyword `yield`.

See below:

```javascript {cmd="node"}
var hello = function *(name){
  yield 'Your name is ' + name;
  return 'Hello ' + name;
}

var gen = hello('James');
console.log(gen.next());
```
The data returned shows an object with the same attributes but returned the new string we just added in our `yield` statement and `done` is false.
We can resume the generator function by calling `next` again on the object:
```javascript {cmd="node"}
var hello = function *(name){
  yield 'Your name is ' + name;
  return 'Hello ' + name;
}

var gen = hello('James');
console.log(gen.next());
console.log(gen.next());
```

As you can see, we can use generators to strip some of the nastiness of callbacks away. Generators cannot be controlled themselves, they must be controlled outside of the generator.
