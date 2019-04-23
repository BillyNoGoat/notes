##JSfuck

Below we see the code used for the letter A.

```javascript {cmd="node"}
var jsFuck = (![]+[])[+!![]];
console.log(jsFuck);
```

We can break this down into simple parts.

We knowe we can force an array into a boolean with the ! operator which expects to have a Boolean so will automatically type convert. This will give us False as an output and this can be reversed by using !! to get true.

```javascript {cmd="node"}
var jsFuck = !![];
console.log(jsFuck);
```

We can then take this and force Javascript to convert from Boolean to Number by using the **unary + symbol**. This is not the + operator for adding two things, it's actually intended to the value on the right side to a number. Applying this to our empty array gives us 0 and we can reverse our boolean to get a 1.

```javascript {cmd="node"}
var jsFuckFalse = +![];
var jsFuckTrue = +!![];
console.log(jsFuckFalse);
console.log(jsFuckTrue);
```

We can then create any number we want by simply creating true to get 1 and adding this with other true/1's until you get numbers 2 - 9.

Now we can deal with numbers as we please, how do we use this to get the letter a? The shortest way is to create the boolean `false` and convert this to a string then take the a from the second position of the word "f**a**lse".

In normal Javascript, we would represent this in the following way:
```javascript {cmd="node"}
var jsFuck = "false"[1];
console.log(jsFuck);
```

So now we need to convert that into jsFuck. We already know how to get the number by making `true` and converting it to a number and we have `[]` available as part of jsfuck so `[+!![]]` makes up the `[1]` we see after the string "false".

So now we need to make up the string "false" to prepend to our `[1]`.

We can do that by adding an empty array to the boolean `false` as this will attempt to concatenate as a string. This looks like: `false+[]`.
But we know we can represent `false` with `![]+[]`.

So now we have a string `false` (Not Boolean) we should be able to add `[1]` on the end to get the character in the string with the index of 1. This actually returns NaN. This is due to the precedence of the operators. To bypass this, we should wrap the `false` expression in `()` as they have the highest precedence of any operator and therefore will ensure `[1]` gets correctly run on the outcome of the initial `![]+[]` string.

See below:

```javascript {cmd="node"}
var jsFuck = (![]+[])[1];
console.log(jsFuck);
```

Now we can simply replace `[1]` with what we did earlier `[+!![]]` to get our final expression for creating "a".

```javascript {cmd="node"}
var jsFuck = (![]+[])[+!![]];
console.log(jsFuck);
```

Now with some combinations we can get all the letters from words `true` and `false`.

We can now start looking for letters elsewhere. We can get `undefined` using `[][[]]` and converting this to a string which gives us 3 letters we didn't get from true or false, `d`, `i` and `n`.

These combinations of letters allow us to spell the words "fill", "filter" and "find". These are all Array methods which can be called on Array literals like `[2,1].sort();`. We can also call methods using square bracked notation instead of dot notation which comes in very useful with jsFuck: `[2,1]["sort"]()`.

```javascript {cmd="node"}
var jsFuck = []["fill"];
console.log(jsFuck);
```

Running this gives us the method header which we can again convert to a string and obtain additional characters from.

In the above instance we get `fill() { [native code] }` which gives us additional characters `c`,`o`,`v`,`(`,`)`,`{`,`[`,`]`,`}`,`.`.

With these additional characters we can now form the word "constructor" which is a method for ALL JS objects have which simply returns their constructor functions which are used to create an object.
