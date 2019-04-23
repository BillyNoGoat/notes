###Why learn functional programming?
Learning functional programming will make you much more efficient. You will be able to write code with less bugs due to the code being easier to reason with and in less time because you can re-use more of your code.

In Javascript, and any functional programming language, functions are values.

For example

```javascript {cmd="node"}
var triple = function(x) {
  return x * 3;
}

var waffle = triple;

console.log(waffle(3));
```

Here we can create an anonymous function and assign that to a variable and call that later.

Functions can also be passed into other functions. Higher order functions.

##Higher-order functions
Higher order functions are good for **composition**.

Look at the example below:

####Filter
One of the most basic higher order functions is filter. It's a function on the array which accepts another function as it's argument which it uses to return a new filtered version of the array.

Below we have an unfiltered array:

```javascript {cmd="node"}
var animals = [
  {name: 'Fluffykins', species: 'rabbit'},
  {name: 'Caro', species: 'dog'},
  {name: 'Hamilton', species: 'dog'},
  {name: 'Harold', species: 'fish'},
  {name: 'Ursula', species: 'cat'},
  {name: 'Jimmy', species: 'fish'}
]

/* Written as a standard for loop - not functional programming.
var dogs = [];
for (var i = 0; i < animals.length; i++) {
  if (animals[i].species === 'dog'){
    dogs.push(animals[i]);
  }
}*/

var dogs = animals.filter(function(animal) {
  return animals.species === 'dog';
})
```
Functions like this filter function (Functions you pass into other functions) are called **callback functions** because the host function will call-back to that. Filter will loop through each item in the array and for each item it will pass it into the callback function. When it does, it will expect the callback function to return either true or false to tell filter wether or not the item should be in the new array. After it's done, it will return the new filtered array which will be dogs only.

These two functions compose together very well. If we breakout our callback function into a variable we can see how this code can be very easily re-used if we break apart the logic.

```javascript {cmd="node"}
var animals = [
  {name: 'Fluffykins', species: 'rabbit'},
  {name: 'Caro', species: 'dog'},
  {name: 'Hamilton', species: 'dog'},
  {name: 'Harold', species: 'fish'},
  {name: 'Ursula', species: 'cat'},
  {name: 'Jimmy', species: 'fish'}
]

var isDog = function(animal) {
  return animal.species === 'dogs';
}

var dogs = animals.filter(isDog);

//Reject is not included in Javascript - It is part of other libraries such as underscore, lodash or mout.
var otherAnimals = animals.reject(isDog);
```

As we can see, we are able to re-use the same function here but applying a different High-order function `reject` to get a list of animals which are NOT dogs.

Compare this to the for loop and you can see we have broken the logic into two sections. The problem of determining if an animal is a dog or not and the problem of creating an array and stuffing the results into them. Using functional programming we are able to deal with the two problems separately and re-use code in functions easily.

We should aim to create small simple functions and composing them together using high-order functions.

We looked at the high order function `filter` but there is a lot that Javascript can offer such as Map and Reduce.

##Map

Previously we learned that `filter` is a method on the array object which takes another function as it's argument and uses that function to filter the array. Here we will learn about the `map` high-order function. Like filter, it goes through an array but instead of throwing the results away, it transforms the elements in the array.

With the following example, we will take the same array and try to find the name of every animal in that array. We will again write the for loop code to demonstrate a non-functional way.

```javascript {cmd="node"}
var animals = [
  {name: 'Fluffykins', species: 'rabbit'},
  {name: 'Caro', species: 'dog'},
  {name: 'Hamilton', species: 'dog'},
  {name: 'Harold', species: 'fish'},
  {name: 'Ursula', species: 'cat'},
  {name: 'Jimmy', species: 'fish'}
]

/*
var names = [];
for (var i = 0; i < animals.length; i++){
  names.push(animals[i].name);
}
*/

var names = animals.map(function(animal) {
  return animal.name;
});

console.log(names);
```
Map will take a callback function just like filter does and the callback function will be passed each item in the animals array. Here is where map is different from filter. Filter expected it's callback function to return a true of false value to show if the item should or shouldn't be in the array.

Map will include all items in the array but expects the callback function to return a transformed object that it will put into the new array instead of the original animal. In this case, this will be the name of the animal.

Using Map to return a subset of an object like this is a very common usage pattern. Since map just expects the callback to return any object, we can use it to create completely new objects too:

```javascript {cmd="node"}
var animals = [
  {name: 'Fluffykins', species: 'rabbit'},
  {name: 'Caro', species: 'dog'},
  {name: 'Hamilton', species: 'dog'},
  {name: 'Harold', species: 'fish'},
  {name: 'Ursula', species: 'cat'},
  {name: 'Jimmy', species: 'fish'}
]

var names = animals.map(function(animal) {
  return animal.name + ' is a ' + animal.species;
});

console.log(names);
```
###Arrow function version
We can combine this with arrow functions to reduce the size of the code even further:

```javascript {cmd="node"}
var animals = [
  {name: 'Fluffykins', species: 'rabbit'},
  {name: 'Caro', species: 'dog'},
  {name: 'Hamilton', species: 'dog'},
  {name: 'Harold', species: 'fish'},
  {name: 'Ursula', species: 'cat'},
  {name: 'Jimmy', species: 'fish'}
]

var names = animals.map(animal => animal.name);

console.log(names);
```
