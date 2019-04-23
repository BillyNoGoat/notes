### Destructuring
One of the most common themes of ES6 is to reduce the amount of duplicate code we see.

Destructuring is about making assignment of object properties a little bit easier to do and without as much duplicate code.

We will do this in the ES5 way and refactor it.

Let's imagine we are making an object that represents an expense. We are going to assign the object properties to variable and we will see that both lines below the object look very similar and have duplicated words multiple times.

```javascript {cmd="node"}
var expense = {
  type: 'Business',
  amount: '$45 USD'
}

var type = expense.type;
var amount = expense.amount;
```

Now let's refactor this into ES6.

```javascript {cmd="node"}
var expense = {
  type: 'Business',
  amount: '$45 USD'
}

const { type } = expense;
const { amount } = expense;
```

The above code is identical to the ES5 version but much cleaner.

When the curly braces are on the left hand side of the `=` it is NOT creating a new object. It is instead creating a new variable and we want it to reference the expense.type property/expense.amount property.

We are just pulling those properties off the object and assigning those to the property of the exact same name.

When we are pulling properties off an object (Destructuring) we can combine these properties into a single line like so:

```javascript {cmd="node"}
var expense = {
  type: 'Business',
  amount: '$45 USD'
}

const { type, amount } = expense;

console.log(type);
console.log(amount);
```

We can now see that we've made two constant variables with the names `type` and `amount` and they are assigned the values of `expense.type` and `expense.amount`.

**NOTE:**
The variable names have to exactly match the properties of the object in order for them to work properly. If we were to use something like `const { myType, amount} = expense;` then myType would return us undefined as there is no property on `expense` called `myType`. Whatever property we want to reference must be identical.

### Advanced example
Let's imagine we have an object which represents a file on our hard drive and we want to print a summary of that.

ES5:

```javascript {cmd="node"}
var savedFile = {
  extension: 'jpg',
  name: 'image',
  size: 14040
};

function fileSummary(file) {
  return `The file ${file.name}.${file.extension} is a size of ${file.size}`;
}

console.log(fileSummary(savedFile));
```
This works but we have pulled out data from `file` 3 times. ES6 will help to reduce this amount.

We can use destructing to pull out the properties so instead of using `file` we can use `{ name, extension, size }` like so:

```javascript {cmd="node"}
var savedFile = {
  extension: 'jpg',
  name: 'image',
  size: 14040
};

function fileSummary( {name, extension, size }) {
  return `The file ${name}.${extension} is a size of ${size}`;
}

console.log(fileSummary(savedFile));
```

Note that it is destructuring from the first object passed in. We could pass in multiple objects and separate our destructured objects with a `,`.

### Destructuing from array.
With destructing we aren't limited to destructure from objects.

Destructing objects is about pulling off properties but destructing arrays is about pulling off elements.

See below:

```javascript {cmd="node"}
const companies = [
  'Google',
  'Facebook',
  'Uber'
];

const [ name, name2, name3, name4] = companies;

console.log(name);
console.log(name2);
console.log(name3);
console.log(name4);
```

As we can see instead of using object we are instead using arrays so in our destructing we use [] before the = operator instead of {}. We should also note that we have created a variable `name4` and as with the objects, as there is no 4th element in the array this simply creates a variable with nothing in it and returns us undefined. It will not throw an error.



### Mixing arrays and objects destructuing
We will make an array of company objects this time and let's say we want to get access the location of google.

ES5:
```javascript {cmd="node"}
const companies = [
  { name: 'Google', location: 'Mountain View' },
  { name: 'Facebook', location: 'Menlo Park' },
  { name: 'Uber', location: 'San Francisco' },
];

var googleLocation = companies[0].location;
console.log(googleLocation);
```
The above code works but is very ugly. We can refactor this in ES6:

```javascript {cmd="node"}
const companies = [
  { name: 'Google', location: 'Mountain View' },
  { name: 'Facebook', location: 'Menlo Park' },
  { name: 'Uber', location: 'San Francisco' },
];

const [{ location }] = companies;
console.log(location);
```
This is a little confusing to understand as there is two sets of destructing going on here so to break this down:

First we start with destructing from the outer most symbol. In this case, it's our `[]` meaning we are destructuring an array.
This will then get us the first element in that array as we would if we were destructuring. Once we have access to that object, the `{}` destructuing takes place on that first array element (the google object).

##### Another example

Let's say we have an object for google containing their locations and we want to get the first location in the locations array of the object.

```javascript {cmd="node"}
const Google = {
  locations: ['Mountain View', 'New York', 'London']
}

const { locations } = Google;

console.log(locations);
```

At this point we are able to destructure our Google object to return us the array. At this point we need to provide directions to Javascript on how to destructure further.

The correct syntax to tell the engine where to look is the following:

```javascript {cmd="node"}
const Google = {
  locations: ['Mountain View', 'New York', 'London']
}

const { locations: [ location ] } = Google;

console.log(location);
```
This is uncommon but this is the syntax if we wished to use it.

### Practical example (objects)

Below let's assume we want to create a new user and save it to our database:

```javascript {cmd="node"}
function signup(username, password) {
  //create new user
}

signup('myName', 'myPassword');
```

This is reasonable code but let's say as time goes by you decide you want users to have to provide their email address, birthday and their living location for example.

```javascript {cmd="node"}
function signup(username, password, email, dateOfBirth, location) {
  //create new user
}

signup('myName', 'myPassword', 'myEmail@example.com', '1/1/1990', 'New York');
```

Now we have a long list of parameters, it becomes pretty taxing to try and remember the exact order of the parameters and ensure that whenever this is called (this could be in another file, or far down the code where you can't easily see what the function takes) that you have put all the right parameters in the right order.

One possible solution is to say instead of passing a list of strings, we could pass an object with all those different properties.

```javascript {cmd="node"}
function signup( { password, email, dateOfBirth, location , username}) {
  //create new user
}

const user = {
  username: 'myname',
  password: 'mypassword',
  email: 'myemail@example.com',
  dateOfBirth: '1/1/1990',
  city: 'New York'
}

signup(user);
```

Now that we're pulling those object properties off and using those as parameters, the order of the parameters no longer matters. The order of destructing does not matter at all.

### Practical example (arrays)
Let's assume we need to plot a graph using x and y axis.

We have data coming to us that looks like:

```javascript {cmd="node"}
const points = [
  [4, 5],
  [10, 1],
  [0, 40]
];
```

But that the data should look instead like:

```javascript {cmd="node"}
const points = [
  {x: 4, y: 5},
  {x: 10, y: 1},
  {x: 0, y: 40}
];
```

We need to somehow take that data of numbers in arrays and turn it into that list of objects on the second example.

My personal test (How I figured it out):
```javascript {cmd="node"}
const points = [
  [4, 5],
  [10, 1, 3],
  [0, 40]
];

const data = points.reduce(function(acc, arr) {
  acc.push({x: arr[0], y:arr[1]});
  return acc;
  }, [])

console.log(data);
```

Instructor ES6 solution:

First way to simply get those x and y values would be:
```javascript {cmd="node"}
const points = [
  [4, 5],
  [10, 1, 3],
  [0, 40]
];

const first = points.map(pair => {
  const x = pair[0];
  const y = pair[1];
})
```
Here is a perfect candidate for some array destructuing:

```javascript {cmd="node"}
const points = [
  [4, 5],
  [10, 1, 3],
  [0, 40]
];

const first = points.map(pair => {
  const [ x, y ] = pair;
})
```
In the above code we are just pulling in the pair object and immidiately pulling the x and y properties off. We can destructure array arguments just as we did with an object aswell. Instead of doing this in the new line, we will do this in the arguments instead:

```javascript {cmd="node"}
const points = [
  [4, 5],
  [10, 1, 3],
  [0, 40]
];

const first = points.map(([ x, y ]) => {
  return { x: x, y: y };
})
console.log(first);
```

*NOTE*: As a reminder, the map function will iterate over every element in an array, run an interator function on those elements and put the returned value into an array and reuturn that array when all elements were itereated over.

Here we can see we iterate over every array of points in the `points` array and destructure each array into two different parameters, x and y.
We then can simply assign these to an object with the x and y key and simply return that object which map will eventually put into an array once it's finished iterating.

Now since we're in ES6 and the object key is the same as the object value, we can shorthand this and reduce this down from `{ x: x, y: y}` to just `{ x, y }`

```
const points = [
  [4, 5],
  [10, 1, 3],
  [0, 40]
];

const first = points.map(([ x, y ]) => {
  return { x, y };
})
console.log(first);
```
