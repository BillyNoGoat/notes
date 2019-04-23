### Rest and Spread
#### Rest

Below we are looking at a typical use of `reduce` where we are summing numbers.

```javascript {cmd="node"}
function addNumbers(numbers) {
  return numbers.reduce((sum, number) =>{
    return sum + number;
    }, 0)
}

const numbersAdded = addNumbers([1,2,3,4,5]);

console.log(numbersAdded);
```

Let's say instead we wanted to pass multiple arguments instead of an array so we would call this with `addNumbers(1,2,3,4,5);`.

We would likely write something like:

```javascript {cmd="node"}
function addNumbers(a,b,c,d,e) {
  var numbers = [a,b,c,d,e];
  return numbers.reduce((sum, number) =>{
    return sum + number;
    }, 0)
}

const numbersAdded = addNumbers(1,2,3,4,5);

console.log(numbersAdded);
```

We have added each number as a parameter then we put those into an array. This creates a fixed length of parameters which isn't useful for us.

The rest operator captures our arguments.

The rest operator syntax is `...numbers`. This basically says we are passing in an unknown number of arguments. We don't know how many there are and we want to capture all those arguments and put them in a single array called `numbers`.

This is used when you want to capture a list of operators.

#### Spread operator
The spread operator is very similar but it's instead used to "flatten" or "spread" the parameters out.

Let's assume we are building an application to display a palette of colours to the users.

```javascript {cmd="node"}
const defaultColours = ['red', 'green'];
const userFavouriteColours = ['orange', 'yellow'];



```

Here we can see we've defined two lists of colours but we want those into a single array. We could use the concat method  `defaultColours.concat(userFavouriteColours)` to do this. We could instead use the `spread` operator to do the same thing.

The syntax for this would be `[... defaultColours, ...userFavouriteColours]`.

The end result is the same as the .concat method. With this operator, we first created a new array (hence the [] it sits in). In front of it we put the spread operator to say we want all the elements in defaultColours and put that into our array and we want to do the same for userFavouriteColours.

It just dumps everything from inside the array to outside the array.

This is almost identical to concat but it is a very clear way of doing this.

Another advantage is that we can easily concat multiple arrays cleanly. We can also add individual elements to the array at the same time we join in other arrays:

`['green', 'blue', ...defaultColours, ...userFavouriteColours]` for example will add multiple elements to the array as well as the inside of the arrays using the spread operator.
