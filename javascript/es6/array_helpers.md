## Array helpers
I have some further notes on each of these but I am adding some extra notes here as a breif reminder.

All of these methods are built around making manual for loops. We should now be done with writing for loops in Javascript using these new array helper methods.

### forEach
Let's assume we have an array of colours and we want to write each one to a console. Classic ES5 code might look something like:

```javascript {cmd="node"}
var colours = ['red', 'blue', 'green'];

for (var i = 0; i < colours.length; i++){
  console.log(colours[i]);
}

```

This is a style we want to avoid for many reasons. It's overly complicated, difficult to debug and non-intuitive. We can also massively shortern it.

We can now use the `forEach` method which is built to help iteration over any iterable object (In this case, an array).

```javascript {cmd="node"}
var colours = ['red', 'blue', 'green'];

colours.forEach(function(colour) {
  console.log(colour);
})
```

We are passing in an anonymous function to our forEach method on the array object which gets called one time per object in the array.

The iterator function is the function we passed into the forEach method. Each object gets run as a parameter to our iterator function and the function executes. It does this one-by-one.


####Advanced forEach
Let's assume we want an array of numbers and sum up the values of the array.

```javascript {cmd="node"}
var numbers = [5,8,3,4];

var sum = 0;

function adder(number){
  sum += number;
}

numbers.forEach(adder);

console.log(sum);
```

Note that we can refer to our `adder` function using just `adder` but we don't invoke it with `()`. This is because it would immediately call `adder` and return the result to `forEach` which is not what we want. Remember that forEach expects a function to be passed into it's parameter which is what we are doing when using `adder` instead of whatever adder returns which would happen if we called `adder()`.

### map
Map is easily the most used array helper around.
We will write some code using a for loop and refactor it using `map`.

Here we will write some code to take an array, double the numbers in that array and put them into a new array.

```javascript {cmd="node"}
var numbers = [1,2,3];
var doubledNumbers = [];

for (var i = 0; i < numbers.length; i++){
  doubledNumbers.push(numbers[i] * 2);
}

console.log(doubledNumbers);
```

The reason we are writing a new array is because in large scale Javascript applications it's best practice not to mutate/change your data. This causes a lot of complexity and introduces the potential for bugs.

Wherever possible we should avoid mutating data and instead return new data.

```javascript {cmd="node"}
var numbers = [1,2,3];

var doubled = numbers.map(function(number){
    return number * 2;
});

console.log(doubled);
```
This is setup similarly to the `forEach` method. We are passing in an anonymous function which gets run for each item in the array.

Whatever is returned in the function we passed in is placed into a new array. After each element is processed, the array is returned. If you don't return a value in the value you pass in, it will simply return undefined.

### filter
Let's assume we have a list of items in a store and we want to filter out only the fruit. Here's how we would do it with a traditional for loop:
```javascript {cmd="node"}
var products = [
  { name: 'cucumber', type: 'vegetable' },
  { name: 'banana', type: 'fruit' },
  { name: 'celery', type: 'vegetable' },
  { name: 'orange', type: 'fruit' }
]

var filteredProducts = [];

for (var i = 0; i < products.length; i++){
  if (products[i].type === 'fruit') {
    filteredProducts.push(products[i]);
  }
}

console.log(filteredProducts);
```
*Note:*
Firstly this is a good example of why we don't want to mutate original data. If we simply removed the data from the original array we would be limited to what we want to do next since we're dumping out our existing data from our application. Instead we're creating a subset of our original data with our filtered results.

`filter` works much like the previous methods. For each object in the array, we will run this against our iterator function passed in as a parameter. If the iterator function returns a `truthy` value then the new resulting array will contain this data, if our iterator function returns a `falsy` value then we do not include it in the new array `filter` will return when all elements in the array have been iterated over.

Refactoring with filter:
```javascript {cmd="node"}
var products = [
  { name: 'cucumber', type: 'vegetable' },
  { name: 'banana', type: 'fruit' },
  { name: 'celery', type: 'vegetable' },
  { name: 'orange', type: 'fruit' }
]

var filteredProducts = products.filter(function(product){
  return product.type === 'fruit';
});

console.log(filteredProducts);
```

### find
The helper's purpose is to search through an array and search for a specific element in an array and return that element.

Below we have the for loop example how we would find an element in an array with the name "Alex".

```javascript {cmd="node"}
var users = [
  { name: 'Jill' },
  { name: 'Alex' },
  { name: 'Bill' }
];

var user;

for (var i = 0; i < users.length; i++){
  if (users[i].name === 'Alex') {
    user = users[i];
    break;
  }
}

console.log(user);
```

To refactor this into the find function:

```javascript {cmd="node"}
var users = [
  { name: 'Jill' },
  { name: 'Alex' },
  { name: 'Bill' }
];

var foundUser = users.find(function(user){
  return user.name === 'Alex';
});

console.log(foundUser);
```

The find method works similarly to `filter` so it runs each element of the array against an iterator function and returns either true or false. If the result is true, it is returned and if false, it moves to the next element in the array.

The find helper will keep calling the iterator function until it returns true or runs out of elements in the array. It will immediately return this value. It returns the FIRST element in the array that matches true and then stops execution (Which is the reason for using `break;` in our for loop example). It will not return multiple values.

### every and some

If we want to find all the computers which have over 16GB of RAM to use a specific program. We have two values defined, one for if every computer can run the program and one for if only some can run the program.
For loop version:
```javascript {cmd="node"}
var computers = [
  { name: 'Apple', ram: 24 },
  { name: 'HP' , ram: 4 },
  { name: 'Acer' , ram: 32 }
];

var allComputersCanRunProgram = true;
var onlySomeComputersCanRunProgram = false;

for (var i = 0; i < computers.length; i++){
    var computer = computers[i];

    if(computer.ram < 16) {
      allComputersCanRunProgram = false;
    }else {
      onlySomeComputersCanRunProgram = true;
    }
}

console.log(allComputersCanRunProgram);
console.log(onlySomeComputersCanRunProgram);
```

#### every
Every will run a check on each object in the array to return either true or false. If any of the elements evaluate to `false` then false will be returned by the `every` method. Only of every element returns true will the `every` helper return true.


ES6 version:
```javascript {cmd="node"}
var computers = [
  { name: 'Apple', ram: 24 },
  { name: 'HP' , ram: 4 },
  { name: 'Acer' , ram: 32 }
];

var allComputersCanRunProgram = computers.every(function(computer) {
  return computer.ram > 16;
});

console.log(allComputersCanRunProgram);
```

#### some
The `some` helper is the reverse of every. It works in the exact same way except instead of returning `true` if all elements return true in the iterator function we pass in, it will return true if ANY of the elements return true in the iterator function.

### reduce
Reduce is a flexible multitool of array methods.

Lets say we want to sum all the numbers in our array.

Here's the for loop version and the reduce version:
```javascript {cmd="node"}
var numbers = [10, 20, 30];

//var sum = 0;

//for (var i = 0; i < numbers.length; i++){
//  sum += numbers[i];
//}

var sum = numbers.reduce(function(sum, number){
  return sum + number;
}, 0);

console.log(sum);
```
Here we have two parameters. One for our iterator function but also a "starting point" which will be used on the FIRST iteration of this array as the FIRST parameter (sum). Then the returned value after an iteration becomes the new value of our first argument (sum).

```javascript {cmd="node"}
var str = '()())('.split('');
var wrongStr = '((((())()))))())'.split('');

var answer = str.reduce(function(acc, char){
  if(char == '('){
    return acc + 1;
  }else if(char == ')'){
    return acc - 1;
  }else{
    return 0;
  }
}, 0);

console.log(answer == 0);
```
