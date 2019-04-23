##Closures

In Javascript, functions are not just functions. They are also closures. What this means is that the function body has access to variables that are defined outside of the function.

```javascript {cmd="node"}
var me = 'Bruce Wayne';
function greet(){
  console.log('Hello, ' + me);
}
greet();
```
As we can see, we are not passing in any parameter to our function `greet`. This is instead using the variable that the function has access to `me` which is sitting in the the outer scope.

In traditional languages, this would not be possible as a function like this would not have access to values in the outer scope of itself. This is because Javascript supports this concept **closures**. In other languages, this would **need** to be passed as a parameter.

###Closure does not copy the variable

It is important to note that the `greet` function does have access to that variable outside of it's scope. It does **not** take a snapshot of the variable `me` at the time of declaration. It does not copy the variable, it actually reads whatever the variable actually is.

For example:

```javascript {cmd="node"}
var me = 'Bruce Wayne';
function greet(){
  console.log('Hello, ' + me);
}
me = 'Batman';
greet();
```
As we can see, this returns us the actual value of the variable at the time of the function being executed.

###Why are closures useful?
If we look at the following function

```javascript {cmd="node"}
function sendRequest() {
  var requestID = '123';
  $.ajax({
    url: '/myUrl',
    success: function(response) {
      alert('Request ' + requestID + ' returned');
    }
  })
}
```
What will happen is that since all functions in Javascript are closures including the callback to `Success` it will have access to the `requestID` which we declared even though this callback is executed way later. This is agood use of closures as we want to start a task and specify something that happens when that task is done with stuff that is available to us when we start the task. Closures makes this easy and readable.

In a language which wouldn't support closures you would probably need some sort of data object which you pass into the ajax function which it would then pass into success and if it didn't have that we'd probably have to make some sort of request object with properties etc.

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures
