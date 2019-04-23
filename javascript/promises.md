#Promises
One of the big things about functional programming is to make your code more composable.
Callbacks are a way of telling your code "then this thing is done, do this". Promises do the same purpose but unlikely callbacks they are much more composable.

###Basic promise example
Look at the below code example:

```javascript
import loadImagePromised from '.load-image-promised'

loadImagePromised('images/cat3.jpg')
  .then((img) => {
    let imgElement =
      document.createElement("img")
    imgElement.src = img.src
    document.body.appendChild(imgElement)
  })
```

In the above example, `loadImagePromised` is being called with the file's location as a parameter.
This function `loadImagePromised` returns a promise object. A promise object has a method on it called `then`. We are calling `then` and passing it a callback function as a parameter.

Once the image is finished loading, the callback will be called with the loaded image `img` and appends this to the body.

The promise object is not the value itself, it is the **promise** of a value. A promise is something you can pass around and write code around it even though you don't have the value just yet.

###Callback version
To contrast, here is the same thing but implemented with callbacks instead of promises:

```javascript
import loadImagePromised from '.load-image-promised'

loadImageCallbacked('images/cat3.jpg', (error, img) => {
  let imgElement =
    document.createElement("img")
  imgElement.src = img.src
  document.body.appendChild(imgElement)
})
```

As with the original code using a promise, we are passing in the file location as our first parameter but the second parameter is a callback. The first argument to our callback function will be an error if there was one. If there isn't one, it will be the success object (In this case the image as `img`).

The function body itself is identical.

This is actually even a bit shorter than the promise pattern so why would we use this?


###Callback hell
Things become more complicated when we start adding more code:

```javascript
import loadImagePromised from '.load-image-callbacked'

let addImg = (src) => {
  let imgElement =
    document.createElement("img")
  imgElement.src = src
  document.body.appendChild(imgElement)
}

loadImageCallBackend('images/cat1.jpg', (error, img1) => {
  addImg(img1.src)
  loadImageCallBackend('images/cat2.jpg', (error, img2) => {
    addImg(img2.src)
    loadImageCallBackend('images/cat2.jpg', (error, img2) => {
      addImg(img3.src)
    })
  })
})

```

As we can see, we are trying to load 3 images asynchronously but keeping the order of the images by using callbacks. What we see here is often refer to as **callback hell** as this code is very ugly and not efficient as the second callback won't even begin being called until the second callback will not be called until the first callback complets meaning we are constantly waiting for our callbacks and the time it takes to complete the whole function and call back completely gets longer and longer.

There is also no error handling here, adding error handling here would add an extra line to each callback function `if(error) throw error;`.

###Inside our asynchronous function
Here we will look inside our callbck function we are importing at the top of the file: `load-image-callbacked`.

```javascript
function loadImage(url, callback) {
  let image = new Image();

  image.onload = function(){
    let message = 'Could not load image at ' + url;
    callback(new Error(msg))
  }

  image.src = url;
}
```
The above code takes two parameters, the url and the callback.
It then creates us anew image object and attempts to load the image and calls back with `null` as the first parameter as `error` is our first parameter and there was no error and with the image as the second parameter (returned).

If there is an error, it hits the onerror method and sends back.

Once it has set those two listeners, it sets the image.src to the url which will trigger the image loading.

We can now re-write this to return a promise as we saw in the beginning.

```javascript
function loadImage(url) {
  return new Promise((resolve, reject) => {
    let image = new Image();

    image.onload = function(){
      resolve(image);
    }

    image.onerror = function() {
      let message = 'Could not load image at ' + url;
      reject(new Error(msg));
    }
    image.src = url.
  })
}
```
The Promise constructor only takes one function as it's arugment. This function/callback will in turn be called with two arguments, resolve and reject. Both of these are also functions. Javascript wants you to call these functions with the values when you have them or when you error.

Insted of calling the callback with the value, we will call `resolve` and pass our data to resolve.

We are expected to call `resolve` with either the success value or calling `reject` with the failure value.

###Promisifying

Now if we go back to our app.js using the callbacks, this will no longer work as we are still looking for objects which no longer exist. We will then promisify the code back:

```javascript
import loadImage from './load-image-callbacked'

let addImg = (src) => {
  let imgElement = document.createElement("img");
  imgElement.src = src;
  document.body.appendChild(imgElement);
}

Promise.all([
  loadImage('images/cat1.jpg'),
  loadImage('images/cat2.jpg'),
  loadImage('images/cat3.jpg'),
  loadImage('images/cat4.jpg')
]).then((images) => {
  images.forEach(img => addImg(img.src))
})
```

As we mentioned earlier, promises help us compose much better than callbacks. Above you can see we have broken our code away from being separate operations and grouped them into an array of promises and execute all at once. We can see from the console.log this is now logging us 3 images.

So when we are calling the loadImages we are passing each promise object to the `.all` method on the promise object. This then returns us a new promise which we are calling `.then` and passing in a callback function and that callback will be called with an array of the actual values that the promises return.

###Promise error handling
We can now simply add a .catch method to the ed of our `.then` method to handle the result:

```javascript
import loadImage from './load-image-callbacked'

let addImg = (src) => {
  let imgElement = document.createElement("img");
  imgElement.src = src;
  document.body.appendChild(imgElement);
}

Promise.all([
  loadImage('images/cat1.jpg'),
  loadImage('images/cat2.jpg'),
  loadImage('images/cat3.jpg'),
  loadImage('images/cat4.jpg')
]).then((images) => {
  images.forEach(img => addImg(img.src))
}).catch((error) => {
  //Error handling is triggered if ANY of the above get triggered.
})
```

###Advanced promise error handling
Example code:

```javascript
function deleteCat(catId) {
  database
  .delete('cats', catId)
  .then(() =>
    console.log('Cat deleted'))
  .catch(err =>
    console.log('Error deleting cat', err))
}
```

A promise is not the value itself but is more an abstract concept that allows us to act as if we had success. It's similar to how you could use a loan promise to buy an apartment. You can say given that the loan resolves to an actual loan, I will buy this apartment.

Now if we
