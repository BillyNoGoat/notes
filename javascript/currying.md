###Currying
Currying is when a function doesn't take all of it's arguments up front. Instead it wants you to give it the first arugment and then the function returns another function which you should call with the second argument which in turn calls another function which you should call with the 3rd function and so on until the final function returns whatever value you want to return.

```javascript {cmd="node"}
let dragon = (name, size, element) =>
  name + ' is a ' +
  size + ' dragon that breathes ' +
  element + '!'

  console.log(dragon('fluffykins', 'tiny', 'lightning'))
```

Here is the same version but the curried version:

```javascript {cmd="node"}
let dragon =
  name =>
    size =>
      element =>
        name + ' is a ' +
        size + ' dragon that breathes ' +
        element + '!'

console.log(dragon('fluffykinds')('tiny')('lightning'));
```
We see that this is a chain of functions. If we remove the last lightning call, we will see we get a response `[function]`.

```javascript {cmd="node"}
let dragon =
  name =>
    size =>
      element =>
        name + ' is a ' +
        size + ' dragon that breathes ' +
        element + '!'

console.log(dragon('fluffykinds')('tiny'));
```

The idea with currying is that your function can pass through the application and gradually recieve the arugments it needs. It goes through your app, you add your arguments as you go and finally you get your output.

For example, we could break out `dragon` into a vairable like so to get the same result:

```javascript {cmd="node"}
let dragon =
  name =>
    size =>
      element =>
        name + ' is a ' +
        size + ' dragon that breathes ' +
        element + '!'

let fluffykinsDragon = dragon('fluffykins');
console.log(fluffykinsDragon('tiny')('lightning'));
```

Or we could even break this out into 3 parts:

```javascript {cmd="node"}
let dragon =
  name =>
    size =>
      element =>
        name + ' is a ' +
        size + ' dragon that breathes ' +
        element + '!'

let fluffykinsDragon = dragon('fluffykins');
let tinyDragon = fluffykinsDragon('tiny');
console.log(tinyDragon('lightning'));
```

###Libraries
This function was written from the start to be curry-able. Every functional library worth it's salt has away to make functions curry-able.

Lodash for example has a curry function (all libraries with any functional programming will have this) and the code would look like:

```javascript {cmd="node"}
import _ from 'lodash'

let dragon = (name, size, element) =>
  name + ' is a ' +
  size + ' dragon that breathes ' +
  element + '!'

dragon = _.curry(dragon)

let fluffykinsDragon = dragon('fluffykins');
let tinyDragon = fluffykinsDragon('tiny');
console.log(tinyDragon('lightning'));
```

###Why is it useful
Example below:
```javascript {cmd="node"}
let dragons = [
  { name: 'flffykins', element: 'lightning' },
  { name: 'noomi', element: 'lightning' },
  { name: 'karo', element: 'fire' },
  { name: 'doomer', element: 'timewarp' }
];

let hasElement = (element, obj) => obj.element === element
let lightningDragons =
  dragons.filter(x => hasElement('lightning', x))

console.log(lightningDragons);

```

So we are calling `filter` on dragons which takes a callback which gets every item. That item will be checked against the hasElement function which checks if it has the element 'lightning' and we pass in the object as `x` and we get a true of false and this will then pass to lightningDragons.

The object that filter iterates `x` becomes the parameter `obj` in our hasElement which gets compared against the element property of the object.

The result filters out fluffykins and noomi out.

We can improve this with currying:

```javascript {cmd="node"}
import _ from 'lodash'

let dragons = [
  { name: 'flffykins', element: 'lightning' },
  { name: 'noomi', element: 'lightning' },
  { name: 'karo', element: 'fire' },
  { name: 'doomer', element: 'timewarp' }
];

let hasElement = _.curry((element, obj) => obj.element === element)

let lightningDragons =
  dragons.filter(hasElement('lightning'))

console.log(lightningDragons);

```
Now `hasElement` is curry-able which means when we call `hasElement` with lightning, it will return a new function which expects to be passed the object to check if it has element `lightning` which allows us to pass it as the callback function to filter directly.

###Conclusion
A curry function is simply a function that takes every argument by itself that then returns a new function that expects the next dependency to the function until all dependcies have been fulfilled and the final value is returned.

We call curry-able functions like `dragon('Karo')('large')('ice')` where we call with the first arugment 9the name) which then returns a new function which we call with the size when in turn returns a new function which we call with the element which will return the final value.
