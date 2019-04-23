## Objects in JavaScript
Unlike other languages like Python and Java, Javascript is a set of standards. Instead of running python with the python compiled or Java applications in JVM, there are different engines to run Javascript. This is possible because it is a set of standards on how Javascript/Ecmascript should run.

This means there is no single decided best way to do things in Javascript so objects can be created in different ways.

```javascript {cmd="node"}
let dog = {
  sound: 'woof',
  talk: function() {
    console.log(this.sound);
    }
}

dog.talk(); // "woof"
let talkFunction = dog.talk
talkFunction() //undefined
```
In the above code, we have created a very simple object with a method. Now we want to pay attention to the variable talkFunction returning us undefined.

Here we took the dog.talk method and re-assigned it to a vairable, we then called the variable and it returned us undefined.

First, let's take note that we can assign a function to a variable. This is possible in Javascript as it is not ONLY an Object oriented language but also a functional programming language.

Functions are values just like strings, numbers etc which means we can assign these to variables without issues.

Now the reason we are getting `undefined` even though the above is true is due to a clash between the object oriented side of Javascript and the functional side of Javascript.

In languages that are functional, there is no concept of `this` or `self` but in object oriented languages, `this` is essential for the language to work.

Now, because we have assigned the variable a **method**, Javascript is taking this method as a face-value **function** meaning it has lost context of the object it belonged to. It has simply taken the **value** directly from the object and assigned it to a variable that is now standalone.

This can be demonstrated by hardcoding the value instead of using `this.` to call it's own property:

```javascript {cmd="node"}
let dog = {
  sound: 'woof',
  talk: function() {
    console.log('woof');
    }
}

dog.talk(); // "woof"
let talkFunction = dog.talk
talkFunction() //undefined
```

### Bind
Here we can use **.bind()** to assign the function to the variable and keep it's context.

```javascript {cmd="node"}
let dog = {
  sound: 'woof',
  talk: function() {
    console.log(this.sound);
    }
}

dog.talk(); // "woof"
let talkFunction = dog.talk
let boundFunction = talkFunction.bind(dog);
boundFunction() //undefined
```

So bind is going to take the `talk` function and return a new function that has bound `dog` to the `this` keyword. `bind` forces `this` to be `dog` in this case.
