#### For of
For of are another way of iterating arrays in ES6.

For of loops are generally better refactored into array methods but have an interesting tie into using Generator functions which is why they are important here.

Below we are iterating through every element of colours so the variable colour will equal the current element being iterated over.


```javascript {cmd="node"}
const colours = ['red', 'green', 'blue'];

for (let colour of colours) {
  console.log(colour);
}

```

This is just a way of iterating through arrays. Looking into Generator functions will help us understand how this works.
