#### NaN

NaN stands for "Not a Number" which has some very strange properties due to how floating point numbers are processed in Javascript.

NaN's type is actually "number" despite being called "Not a Number" which is very confusing. It basically represents any mathematical calculation that could not be properly processed such as finding the square root of a negative number or dividing 0 by 0.

NaN is not equal to, greater than or less than anything including itself. This is because of the IEEE755 floating point standard which is a non-javascript thing which tells us there are 16,777,214 different floating point values as NaNs. This means ther are so many different representations of NaN that it's highly unlikely to be the same NaN.
```javascript {cmd="node"}
console.log(typeof(NaN));
console.log(Math.sqrt(-5));
console.log(NaN == NaN);
console.log(isNaN(NaN));
```
The only proper way to tell if a number is NaN or not is to use the built in Javascript function isNan();

#### Sorting Arrays
```javascript {cmd="node"}
var myArray = [33,2,98,25,4];
console.log(myArray.sort());
```
Here we can see that sort is not sorting from smallest to largest number. This is because Javascript is using Lexicographical sorting which is dictionary/alphabetical and not numerical. This means Javascript will attempt to convert all integers to strings which changes them to unicode values. For every character in those strings, it is comparing the unicode value.

The output becomes [**2**,**2**5,**3**3,**4**,**9** 8].
The parts highlighted in bold are how these are being sorted. We can see the unicode values of a specific position (The first position is at 0) by using the following:
```javascript {cmd="node"}
console.log("25".codePointAt(0));
```
The sort method can actually take a parameter for a compare function:
```javascript {cmd="node"}
function compare (a,b) {
  if (a < b) return -1; //a comes first then b
  else if (a > b) return 1; //b comes first then a
  else return 0 //a and b are left unchanged
}
```

Now we have a compare function, we can finally have a numerical sort:
```javascript {cmd="node"}
var myArray = [33,2,98,25,4];
console.log(myArray.sort((a,b) => a-b));
```

#### Tilde operator ~
This is a bitwise operator. A bitwise operator is an operator where we take a number and it gets transformed into bits, we run an operation on it and it returns a result.

The **~** is a **Bitwise NOT** operator which in short will reverse the bits in a sequence:
```
9 = 00000000000000000000000000001001
~9 = 11111111111111111111111111110110
```
This can be useful for truncating numbers faster than `Math.trunk` or `Math.floor` and very shorthand:
```javascript {cmd="node"}
console.log(~~1.2543);
console.log(~~4.9);
console.log(~~-2.999);
```
Another thing we can use the bitwise NOT is with an Array.
We can use `indexOf` on an Array to get the position of a value in an array. This will return a -1 if there is not this element in an array which is often used to check if an element exists.
This can be much more simplified using the ~ operator to check if an element exists (Although ES7 will bring us an `includes` function to see if an element exists ot not).
```javascript {cmd="node"}
var arr = ["blue", "red", "green"];
if(~arr.indexOf("purple")){
  console.log("Exists");
  }else{
    console.log("Doesn't exist");
  }
```
This saves us checking the value of the value against -1/0 to determine how to handle it. What is happening is that the element was not found, a -1 was given to use and then the tilde operator will convert this to a `0` which is considered false.

#### For loops
`for(;;){}`
For loops have 3 parts. An initialization part, a condition part and an iteration part:
`for (initialization; condition; iteration;){//code here}`
All of these parts are optional in Javascript.
* You can remove **Initialization** as we can declare variables outside of the for loop as this does not matter.
* You can remove the **Condition** as Javascript will automatically consider it true and it's up to you to break the loop.
* You can remove the **Iteration** part away as you can with Initialization. Once this drops to 0, the loop will stop.

Or you can take it all out and have an infinite loop.

```
for(;;) {} = for(; true ;) = while(true) {}
```

####JSFuck
##### Everything in Javascript is truthy unless it's falsy
Falsey values are:
* False
* 0
* ""
* null
* undefined
* NaN

Everything else in Javascript is considered true including:
* "0"
* "false"
* []
* {}




#### Incomplete/Other irregularities
```javascript {cmd="node"}
console.log(Math.max()); //-Infinity
console.log(Math.min()); //Infinity

console.log([1,2,3] === [1,2,3]); //false
```
