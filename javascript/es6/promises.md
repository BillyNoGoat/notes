### Promises
#### ES6 Promises
Promises have actually been around in Javascript for years but have not had an official implementation. These promises were implemented in modules such as axios, bluebird etc but ES6 brings official implementations for promises.
These will now be 100% supported in libraries and browsers as we now have a native implementation.

##### Pausing execution in Javascript
There is no such concept as pausing code in Javascript. In some languages there exists things like `sleep(1000)` to pause execution on a single line for a certain amount of time. There are some functions in Javascript to *defer* execution such as `setTimeout()`.

#### HTTP example
Let's say we're doing an AJAX request which is an HTTP network reqeuest. Any network request that goes out to the internet will take a non-zero amount of time to complete and is impossible to predict the amount of time taken to get the response back.

Now if we tried to console.log immidiately after making an HTTP request without waiting for any response, we will likely get undefined as the response hasn't come back yet.

This is what promises are trying to solve. A situation where a long running operation returns a result that we would like to access.

What we would like is to be able to make a request out to the internet and only **THEN** after the request has resolved do we want to process that information.

#### Terminology
In order to determine the state of long running process we need to understand the 3 states a promise could exist in.
- Unresolved: Waiting for something to finish...
- Resolved: Something finished and it all completed without any errors.
- Rejected: Something finished and went wrong.

Unresolved will not need to be handled in any way other than to delay the execution of a callback. Until it's resolved, we should do nothing that requires it to be completed.

For a resolved promise, we will need to **then** callback and execute whatever else we needed to execute.

For a rejected promise, our callback should **catch** the error and handle this properly.

**.then** and **catch** are properties on a promise object which we use for this flow.

### Creating Promises
Our goal here is to create a promise.

**NOTE:**
Promises are an entire subject that could be discussed on many things other than AJAX requests but AJAX requests are the most widely used ways that promises are used. You can make an AJAX request without a Promise and you can make a Promise without an AJAX request - they just work nicely together but Promises are useful for many things. They are not mutually dependant.

Now let's create a simple promise object. We will need to pass in a function to the constructor for this Promise object.
Our function will be called with two arguments `resolve` and `reject`. These are both **functions**. This means we can call them in our code using `resolve();` or `reject();`. Calling one of these functions will change the value of the promise (**NOTE** this is why we use let instead of const). This is the purpose of `resolve` and `reject`.

```javascript {cmd="node"}
let promise = new Promise((resolve, reject) => {
  resolve();
});

console.log(promise);
```

Below we successfully create a promise and call the `resolve()` function that gets passed to us by the constructor of the Promise object.

Now let's run this with the `reject()` function instead.

```javascript {cmd="node"}
let promise = new Promise((resolve, reject) => {
  reject();
});

console.log(promise);
```

Note running the above now gives us an error
`Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). (rejection id: 1)`

This basically means that the promise was rejected but that we didn't handle or catch the error which is always required.

#### .then and .catch
Let's make use of our promise practically. If you recall, we said when a promise is resolved, any callbacks we registered with `then` will be called and when a promise is rejected, any callbacks we registered with `catch` will be resolved.

We will be using `.then()` and `.catch()` to register callbacks. See below we pass in a function as a parameter:

```javascript {cmd="node"}
let promise = new Promise((resolve, reject) => {
  reject();
});

promise.then(() => {
  console.log('finally finished!');
})

console.log(promise);
```
See in the above example that we registered a callback using only `then()` which will only get called if the promise is `resolved`. In this instance, we rejected it and therefore we get the same error as before and we see no sign of our "Finally finished" message in our `then()` callback function.

Now let's see how this works when set to `resolve`:

```javascript {cmd="node"}
let promise = new Promise((resolve, reject) => {
  resolve();
});

promise.then(() => {
  console.log('finally finished!');
})

console.log(promise);
```
It worked fine and logged as we expected.

Whenever we create a promise like this, we can register multiple functions so we can chain on multiple functions with .then for a single `resolve`. These are two separate callbacks for a single promise object.

Note we have also refactored the arrow functions to not use curly braces (As nothing is returned) and remove semicolon for short hand.

```javascript {cmd="node"}
let promise = new Promise((resolve, reject) => {
  resolve();
});

promise
  .then(() => console.log('finally finished!'))
  .then(() => console.log("I was also run"))
```

Now we can look to handle a reject using `.catch()`

```javascript {cmd="node"}
let promise = new Promise((resolve, reject) => {
  reject();
});

promise
  .then(() => console.log('finally finished!'))
  .then(() => console.log("I was also run"))
  .catch(() => console.log("The promise got rejected!"))
```
Now note that the `.then` never runs and instead we instantly hit the `catch` statement.

Also note that because we registered a `catch` statement we can see the red error about not catching is now gone.

#### Practical applications of a promise
Now we can finally understand how promises can answer issues with asynchronous code.

Inside of a promise, everything still falls back to the browser to tell us when an AJAX request has been completed.

Below let's simulate a long running process which lasts 3 seconds. We will simulate this with setTimeout.

```javascript {cmd="node"}
let promise = new Promise((resolve, reject) => {
  setTimeout(() => {
      resolve();
    }, 3000);
});

promise
  .then(() => console.log('finally finished!'))
  .then(() => console.log("I was also run"))
  .catch(() => console.log("The promise got rejected!"))
```

We can do the exact same thing with `reject` if we wish. Internally this is how an AJAX request is handled too.


#### Fetch
**NOTE:**
This is a BROWSER implementation but the points still stand for many other asynchronous tasks.

Now we have our eyes on Promises, we can look at `fetch`. Just like promises, this object is available natively to Javascript.
If we were to log out `fetch` in our browser we would see it's a function and see the `[native code]` string to say it's part of the browser.

Let's do an example:

```javascript {cmd="node"}
const url = "https://jsonplaceholder.typicode.com/posts/";
console.log(fetch(url));
```

Here we can see that this returns us a promise object. Now let's register a .then helper for this.
We know that fetch will return us some amount of data so let's log this data to the console:

```javascript {cmd="node"}
const url = "https://jsonplaceholder.typicode.com/posts/";
fetch(url)
  .then(data => console.log(data))
```

Notice some of the properties on the fetch helper data. These are normal for fetch.

Note that there's no property here that contains the JSON data that we got back from the server. When you make a request, the response you get back is actually an object that represents the response we got back. To get the data, we need to call a method on it.

In this case, we want to call `.json()` on our object:

```javascript {cmd="node"}
const url = "https://jsonplaceholder.typicode.com/posts/";
fetch(url)
  .then(response => response.json())
  .then(data => console.log(data))
```

Only after we request `response.json` do we get a json response in our next `.then`.

**NOTE**
AJAX/Jquery/Axios(AJAX) and other methods are generally preferred over using Fetch.
