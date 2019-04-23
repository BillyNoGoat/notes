#Getting the letter A.

##Getting True
First we can start with understanding how typecasting works in a dynamic language.

So we understand that we can force an array into a Boolean with the `!` operator since `!` expects to have a Boolean so will automatically type convert and since the array is empty, it considers this a **false value** and therefore return false when using `![]`. To make this true, we simply reverse it as you can do with any Boolean like `!true` to return false. It's just a double negative to say it's not-not true (false).

```javascript {cmd="node"}
var jsFuck = !![];
console.log(jsFuck);
```

##Getting numbers 0 - 9
We can then take this and force Javascript to convert from Boolean to Number by using the **unary + symbol**.
This is not the + operator for adding two things, it's actually intended to the value on the right side to a number.
Since it expects a number as a value and as we've already seen, Javascript interprets `![]` as `false`. Javascript tries to be clever and sees you're asking for a number but you have only the booleans True and False, so it is built to convert these to `1` for true and `0` for false.

```javascript {cmd="node"}
var jsFuckFalse = +![];
var jsFuckTrue = +!![];
console.log(jsFuckFalse);
console.log(jsFuckTrue);
```

We can see the same code above represented as:
```javascript {cmd="node"}
var jsFuckFalse = +false;
var jsFuckTrue = +true;
console.log(jsFuckFalse);
console.log(jsFuckTrue);
```

or

```javascript {cmd="node"}
var jsFuckFalse = Number(false);
var jsFuckTrue = Number(true);
console.log(jsFuckFalse);
console.log(jsFuckTrue);
```

We can then create any number we want by simply by creating the number 1 and using the + operator to get numbers 0-9. This allows us to create literally any whole number.

##Getting selected letters

Now we have numbers at our disposal, what can we do with them?

We can use these to get letters by understanding that strings are considered "Array-like" which is a concept I will let you look into yourself. In short, it means we can treat them as arrays of individual characters which means we can access any character in a string by simply taking a string and asking for a specific element at a specific index. For example if we use `False` which we already know how to make, we can then use `[]` on that string to get a specific character at a specific index just as we would with an array.

In normal Javascript, we would represent this in the following way:
```javascript {cmd="node"}
var jsFuck = "false"[1];
console.log(jsFuck);
```

To recap what we need to do to write in JSFuck:
- We know how to make the Boolean `false` by making an empty array and converting it into a Boolean using `!`
- We already know how to get the number `1` by making `true` and converting it to a number.
- We already have the characters `[]` available to us as they are 2 of the 6 allowed characters!

The only new challenge to overcome is turning a Boolean into a String so we can access each character element in the string by it's index value.
This can be overcome simply by adding an empty array to the Boolean `![]+[]`

So now we have a string `false` (Not Boolean) we should be able to add `[1]` on the end to get the character in the string with the index of 1. This actually returns NaN. This is due to the precedence of the operators. To bypass this, we should wrap the `false` expression in `()` as they have the highest precedence of any operator and therefore will ensure `[1]` gets correctly run on the outcome of the initial `![]+[]` string.

Now we just need to make each element and slot them together to mimic the last example.

See below we have created false as a Boolean, converted it to a string and then selected the character in the Array with an index of `1`:

```javascript {cmd="node"}
var jsFuck = (![]+[])[1];
console.log(jsFuck);
```

Now we can simply replace `[1]` with what we did earlier `[+!![]]` to get our final expression for creating "a".

```javascript {cmd="node"}
var jsFuck = (![]+[])[+!![]];
console.log(jsFuck);
```

Now by switching out the Boolean from False to True and switching the index from 1 to other numbers we can get any f the letters from `true` and `false`!

##Getting the rest of the letters

We can now start looking for letters elsewhere There are a few responses in Javascript which return string responses or something we can turn into a string. We can get `undefined` using `[][[]]` and as undefined is actually an object in Javascript, we convert this to a string which gives us 3 letters we didn't get from true or false, `d`, `i` and `n`.

These combinations of letters allow us to spell the words "fill", "filter" and "find". These are all Array methods which can be called on Array literals like `[2,1].sort();`. We can also call methods using square bracket notation instead of dot notation which comes in very useful with jsFuck: `[2,1]["sort"]()`.

```javascript {cmd="node"}
var jsFuck = []["fill"];
console.log(jsFuck);
```

Running this gives us the method header which we can again convert to a string and obtain additional characters from.

So far though, we get a return of `fill() { [native code] }` which gives us additional characters `c`,`o`,`v`,`(`,`)`,`{`,`[`,`]`,`}`,`.`.

With these additional characters we can now form the word "constructor" which is a method for ALL JS objects have which simply returns their constructor functions which are used to create an object.

See the code below:

```javascript {cmd="node"}

console.log(true["constructor"] + []);
console.log(0["constructor"] + []);
console.log(""["constructor"] + []);
console.log([]["constructor"] + []);
```

Here we can see we can get strings containing other characters we didn't previously have:
`B`,`N`,`S`,`A`,`m`,`g`,`y`.

From this we can create the string `toString`. As `toString` is simply a method, we can access it with square bracket notation just like any other method and we can invoke the function using `()`.

See the code below:

```javascript {cmd="node"}
var jsFuck = (10)["toString"]() === "10"
console.log(jsFuck);
```

But we already know how to convert to strings so this method is not going to be used to convert to strings. Instead we will make use of it's second parameter called `radix` which changes the base of the returned number before it's converted to a string. See below to see outputs of numbers in base 10, base 2, base 8, base 16 (hex) and most importantly for us, base 32 (used for encoding into ASCII/Characters):

```javascript {cmd="node"}
console.log((12)["toString"](10));
console.log((12)["toString"](2));
console.log((12)["toString"](8));
console.log((12)["toString"](16));

console.log("\n---We can now use base-32 to make any character we want---\n");

console.log((10)["toString"](32));
console.log((11)["toString"](32));
console.log((12)["toString"](32));
```
