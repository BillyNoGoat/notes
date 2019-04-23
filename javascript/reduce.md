###Reduce
Reduce is another high order function.

To recap on previous higher order functions, there are map, filter and reject. They transform a list and turn it into something else.
- **Map** will take an array and transform each object in the array to have an equal length but with each item transformed
- **Filter** transforms an Array into a smaller array.
- **Reject** does the same thing as filter but the reverse.
- **Find** was not mentioned but does the same thing as **filter** but only returns the first found. This transforms an array into a single object.

If you cannot find one of these which fits, we can use **Reduce**. Reduce is non-specific and can be used to express any list transformation. We can use **reduce** to implement any of the above list transformations. We can always fall back on reduce if none of the pre-built methods listed above fit what you need.

```javascript {cmd="node"}
var orders = [
  {amount: 250},
  {amount: 400},
  {amount: 100},
  {amount: 325}
]

var totalAmount = orders.reduce(function(sum, order) {
  return sum + order.amount;
  }, 0);

/* How this would be done with a for loop.
var totalAmount = 0
for (var i = 0; i < orders.length; i++){
  totalAmount += orders[i].amount
}*/

console.log(totalAmount);
```
Just like `Map` and `Filter` it takes a callback function but unlike these two methods, it also wants an object. This object can be conisdered a starting point for our sum which will be `0`. It will be the first argument for the callback function `sum`. It also requires the list as an argument which is the second arugment `order`.

We then take the initial value of `sum` and add the order amount to it. That return value will then in turn be passed as the sum to the next iteration and will loop through until we are finished.

###Arrow function version:
```javascript {cmd="node"}
var orders = [
  {amount: 250},
  {amount: 400},
  {amount: 100},
  {amount: 325}
]

var totalAmount = orders.reduce((sum, order) => sum + order.amount, 0);

console.log(totalAmount);
```


###Advanced Reduce
```javascript {cmd="node"}
var fs = require('fs');
var output = fs.readFileSync('resources/reduce/reduceData.txt', 'utf8')
  .trim()
  .split('\n')
  .map(line => line.split('\t'))
  .reduce((customers, line) => {
    //console.log(line);
    customers[line[0]] = customers[line[0]] || []
    customers[line[0]].push({
      name: line[1],
      price: line[2],
      quantity: line[3]
      })
    return customers;
    })
console.log('output', JSON.stringify(output, null, 2));
```

Below we are loading the file in utf8 format and splitting this into an Array at the new line.
```javascript {cmd="node"}
var fs = require('fs');
var output = fs.readFileSync('resources/reduce/reduceData.txt', 'utf8')
  .split('\n')
console.log(output);
```

There is an extra line here on the bottom which we do not want. Files tend to have extra lines on the end of them. As such we can chain .trim to the end to remove any whitespace at the end or start of a string.

We will then make this into a more managable object by using `map`.

We will map every line and we will split it on tab characters. This means instead of having an array which contains one element per line as a long string, we will have an array containing an array for each line which has elements for each value split by a tab. See output below:

```javascript {cmd="node"}
var fs = require('fs');
var output = fs.readFileSync('resources/reduce/reduceData.txt', 'utf8')
  .trim()
  .split('\n')
  .map(line => line.split('\t'))
console.log(output);
```

We can now use reduce.

Remember that `reduce` takes two arguments. First it takes a callback function and second it also takes a starting object. Previously we used 0 as our starting object. In this case we will create an object literal.

The callback function that was pass in wants 2 arguments. The first is the object we are constructing (The end goal) which will be our object lieral. The second argument will be the thing we are iterating which will be the line. The line being the current array that's being passed in (Since we have an array of arrays, it loops through and passes in each array).

```javascript {cmd="node"}
var fs = require('fs');
var output = fs.readFileSync('resources/reduce/reduceData.txt', 'utf8')
  .trim()
  .split('\n')
  .map(line => line.split('\t'))
  .reduce((customer, line) => {

  }, {})

console.log(output);
```
If we run this, we can see we get an output of `undefined`. This is because we are not returning any value from our `reduce` and therefore `output` will be whatever reduce returns on it's final iteration.

So let's return the customer object.

Let's also console.log the line out so we can see what Javascript sees on each iteration in reduce.

```javascript {cmd="node"}
var fs = require('fs');
var output = fs.readFileSync('resources/reduce/reduceData.txt', 'utf8')
  .trim()
  .split('\n')
  .map(line => line.split('\t'))
  .reduce((customer, line) => {
    console.log('LINE: ', line);
    return customer;
  }, {})

console.log(output);
```

We can see the output of each line on every iteration of the reduce function and can also see the empty object that exists in `output`. That is because we aren't doing anything with the returned customer yet.

Now let's do something with the data we return. For every customer in the array we want to make a property with their name. Remember the first element in the array (index 0) is the name:

```javascript {cmd="node"}
var fs = require('fs');
var output = fs.readFileSync('resources/reduce/reduceData.txt', 'utf8')
  .trim()
  .split('\n')
  .map(line => line.split('\t'))
  .reduce((customer, line) => {
    customer[line[0]] = [];
    return customer;
  }, {})

console.log(output);
```
We can see we have now one empty array for Mark Johnson and one empty array for Nikita Smith. This is because even though it will get each line one by one, it will simply overwrite the Mark Johnson and Nikita Smith property on each iteration.

We will fix this later.

First we will focus on pushing additional attributes to the objects on each iteration to capture all the relevant data into objects.

We will also stringify the output into JSON with 2 spaces between the JSON to make it more readable:

```javascript {cmd="node"}
var fs = require('fs');
var output = fs.readFileSync('resources/reduce/reduceData.txt', 'utf8')
  .trim()
  .split('\n')
  .map(line => line.split('\t'))
  .reduce((customer, line) => {
    customer[line[0]] = [];
    customer[line[0]].push({
      name: line[1],
      price: line[2],
      quantity: line[3]
      })
    return customer;
  }, {})

console.log(JSON.stringify(output, null, 2));
```
Now lastly we just need to fix the bug with the last item for each person being shown. As mentioned previously, it's because we are overwriting this when we create the array.
We can simply fix this by changing the line
From: `customer[line[0]] = [];`
To: `customer[line[0]] = customer[line[0]] || [];`

This tells us either re-use the existing array if one exists otherwise assign a new one.

```javascript {cmd="node"}
var fs = require('fs');
var output = fs.readFileSync('resources/reduce/reduceData.txt', 'utf8')
  .trim()
  .split('\n')
  .map(line => line.split('\t'))
  .reduce((customer, line) => {
    customer[line[0]] = customer[line[0]] || [];
    customer[line[0]].push({
      name: line[1],
      price: line[2],
      quantity: line[3]
      })
    return customer;
  }, {})

console.log(JSON.stringify(output, null, 2));
```

 And now the job is complete and we have successfully converted out tab separated file into javascript objects grouped per person in the order.

 We can now look at a shorter ES6 way of doing this:

```javascript {cmd="node"}
var fs = require('fs');
const output = fs.readFileSync('resources/reduce/reduceData.txt', 'utf8')
  .trim()
  .split('\n')
  .map((line) => line.split('\t'))
  .reduce((customers, [name, order, price, quantity]) => {
    customers[name] = customers[name] || [];
    customers[name].push({ order, price, quantity });
    return customers;
  }, {});﻿
console.log(JSON.stringify(output, null, 2));
```


###Simple example:
Examine the code below:
```javascript {cmd="node"}
var array = [1,2,3,4,5];
console.log(get(array));

function get(array) {
    return array.reduce(function(a,b){
        a //1, 3, 6, 10
        b //2, 3, 4, 5
        return a + b; //15
    });
  };
```

As we can see, the value of `a` becomes the accumulative value after the first run (When values are calculated) and we continue to loop through the array until we can reduce it to a single value of `15`. b stays as individual untransformed elements in the array which is just being used per iteration on the accumulative value stored in `a`. Each coma separated value in the comments shows the value of each variable on each iteration.

Now this could be done with a simple loop but what makes this very useful is that instead of providing just a callback as a parameter for reduce, we can also add a second parameter called the `initialValue`. This second argument will be passed in as `a` the first time if it's provided.

Examine below:
```javascript {cmd="node"}
var array = [1,2,3,4,5,7,8,9];
console.log(get(array));

function get(array) {
    return array.reduce(function(a,b){
        a[b] = b * b; //?
        return a;
    }, {});
  };
```
Here we have provided an empty array as our second parameter to reduce. This then becomes the value of `a` for the first run and after the first run, a continues as normal as the result of the iteration:

```
1ST RUN: A = {}, B = ARRAY[0]
2ND RUN: A = 1ST RESULT, B = ARRAY[1]
3RD RUN: A = 2ND RESULT, B = ARRAY[2]
```

So the return value becomes an object of each number's square root. In our callback function, we are setting `a` (The object)'s index to the value of that index times itself (It's root) and returning a to be used for the next iteration in the value of `a`.

```javascript {cmd="node"}

var orders = [
    {amount: 250},
    {amount: 450},
    {amount: 750},
    {amount: 120},
  ];

var sumOrder = orders.reduce(function(a, b){
    return a + b.amount; //?
}, 0);

console.log(sumOrder);
```
