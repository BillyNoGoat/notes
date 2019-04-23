### What is a generator?
A generator is a function that can be enetered and exited multiple times. Normally when we run a function, it runs and normally returns a value and exits.
With generator functions we can return a value in a function and even go back into our function and continue running.

Below we will focus on what the syntax will look like.

To make a generator function we need 1 single character which is to add `*` after the `function` keyword. Either `funtion* numbers(){}` or `function *numbers(){}`.

Below is an example of a generator:

```javascript {cmd="node"}
function* numbers() {
  yield;
}

const gen = numbers();

console.log(gen.next());
console.log(gen.next());
```

### What does a generator do?
Let's examine what is going on in the above code.

We can see that the two times we call `gen.next();` we are getting different objects back. One with `done: false` and one with `done: true`.

Inside the generator function we are using the `yield` keyword.

Let's pretend we are writing a function to match a story.
You are at home and you're hungry and you need to go to the store to buy food and you head home:

We should make note of the distinct transition of going INTO a store with money and coming OUT the store with groceries.
The store and outside the store should be considered as two different contexts in our story and will be treated separately. We will use two comments to specify how we are separating these two contexts in our code too.

```javascript {cmd="node"}
function* shopping() {
  // (Outside the store)

  //walking down the sidewalk

  //go into the store with cash
  const stuffFromStore = yield 'cash';

  // walking back home
  return stuffFromStore
}
// (Inside the store)

const gen = shopping();

console.log(gen.next()); // Leaving our house
// Walked into the store
// Purchasing our stuff

console.log(gen.next('groceries')); // Leaving the store with groceries
```

First we are leaving our house with our first call to `gen.next();`. When we are creating the constant `gen` although we are calling `shopping()` no code gets invoked whatsoever. It's not until we call `gen.next()` then we start executing and start "walking down the sidewalk".

Now as we can see in the comments, the context of the sidewalk is within our function and outside of that is in the store.
Here once we hit our first yield we are stopping execution in that context and being put back into our main context of being in the store and we are yielding with `'cash'`.
We are pausing execution in our generator function.

Now when we call .next again on our generator function, we are basically going back into that context (back into our generator). So calling gen.next() again will re-enter our function back to run after our last yield statement.

So on our second `gen.next('groceries')` we can see we've passed in a parameter which becomes the value of `stuffFromStore` in our generator function.

In summary, we entered our generator function calling .next on the generator function and yielded (paused execution) passing out the value of `cash` on our generator object (see the console.logs). We then called back into that function again using the .next method and this time passed in our own data to become the value of `stuffFromStore`. We then returned this back out to the main context (again, see console log).

The `done` property determines if there is anything left to run in our generator function.

For every `yield` we need to call a `.next()`.

#### Why use them?
Generators work perfectly with `for of` loops.

As you can see below, every single time we yield in a generator, it creates a single run of our `for of` loop. The for of loop runs 3 times here and in each iteration of our loop, colour will be equal to the value of the individual colour red, blue, green etc.

This is the real reason to be using generator functions. Here we can iterate through any data structure we want.
```javascript {cmd="node"}
function* colours(){
  yield 'red';
  yield 'blue';
  yield 'green';
}

const gen = colours();

const myColours = [];
for (let colour of colours()){
  myColours.push(colour);
}

console.log(myColours);
```

#### Practical example
Now we can use generators to figure out how to iterate through the engineeringTeam object for the team members. We can't iterate through all keys in the object as not all properties of the object are for team members. We are not interested in the size or department.

We can make a multi-step generator to handle this.
```javascript {cmd="node"}
const engineeringTeam = {
  size: 3,
  department: 'Engineering',
  lead: 'Jill',
  manager: 'Alex',
  engineer: 'Dave'
};

function* teamIterator(team) {
  yield team.lead;
  yield team.manager;
  yield team.engineer;
}

const names = [];
for (let name of teamIterator(engineeringTeam)) {
  names.push(name);
}

console.log(names);
```

The above is a very good use-case for iterating over particular properties of an object.

Note we could re-use this function and pass in another team object if we created one which would pull out the same information.

### Generator delegation
Let's assume we have an engineering team made up of a lead, manager, engineer and another team called the testing team which itself will contain a lead and a tester.

The testing team belongs to the engineering team but has it's own lead and tester.

If we want to iterate through both teams and get all the teams how might we do that?

**NOTE:**
In the below code, the testingTeam property is following ES6 syntax. Instead of having a property on engineeringTeam called `testingTeam: testingTeam` we since the key and value are the same we can just use `testingTeam` and we will move it to the top as is best practice when using this shorter syntax (They go to the top).

```javascript {cmd="node"}
const engineeringTeam = {
  testingTeam,
  size: 3,
  department: 'Engineering',
  lead: 'Jill',
  manager: 'Alex',
  engineer: 'Dave'
};

const testingTeam = {
  lead: 'Amanda',
  tester: 'Bill'
}

function* teamIterator(team) {
  yield team.lead;
  yield team.manager;
  yield team.engineer;
}

function* testingTeamIterator(team) {
  yield team.lead;
  yield team.tester;
}

const names = [];
for (let name of teamIterator(engineeringTeam)) {
  names.push(name);
}

console.log(names);
```
Now, in our new code, we need to ensure that our generator function will iterate over both the testingTeam and the engineeringTeam.

Theres two ways.
We could change the existing iterator to do `yield team.testingTeam.lead;` but a more reusable and sustainable solution would be to make a new iterator/generator to go over the testingTeam (this also means we can iterate over just the testing team if we wished).

We added this can called it testingTeamIterator. The next issue we've had to deal with is to ensure that the for loop iterates over both the engineeringTeam and the testingTeam.

#### Delegating
The entire practice of starting in one area and attempting to move that iteration down to another area is called iterator delegation. We are delegating down from the engineeringTeam iterator down to the testingTeam iterator.

We can delegate down to our testingTeamIterator by simply adding to our teamIterator the line`const testingTeamGenerator = testingTeamIterator(team.testingTeam);`.
Team of course refers to the team we've passed in which is our engineering team and we're passing in the testingTeam to our testingTeamIterator.

Now we also need to ensure both yield statements in the second generator are correctly yielded. To ensure the `for of` loop correctly iterates over these yields, we need to add to our code `yield* testingTeamGenerator;`.

```javascript {cmd="node"}
const engineeringTeam = {
  testingTeam,
  size: 3,
  department: 'Engineering',
  lead: 'Jill',
  manager: 'Alex',
  engineer: 'Dave'
};

const testingTeam = {
  lead: 'Amanda',
  tester: 'Bill'
}

function* teamIterator(team) {
  yield team.lead;
  yield team.manager;
  yield team.engineer;
  const testingTeamGenerator = testingTeamIterator(team.testingTeam);
  yield* testingTeamGenerator;
}

function* testingTeamIterator(team) {
  yield team.lead;
  yield team.tester;
}

const names = [];
for (let name of teamIterator(engineeringTeam)) {
  names.push(name);
}

console.log(names);
```
Explaining this code in full:
When we use generators for iteration, we first create the generator function which we're calling here an `iterator`. In this case, this is our `function* teamIterator(team) {}`.
First we call our teamIterator and we pass in the testing team. This then creates our generator. Now we need to ensure that our `for of` loop knows that it has a few yield statements of it's own that it needs to take into account which is where `yield*` is for.
We can think of `yield*` as "We are in a generator but I have another generator which has a few yield statements in it you might care about.". We can think of this as a trap door which will trick the for of loop into falling through. Once it falls through, it falls into the testingTeamInterator and continues to iterate through the new generator function before coming back up.

This is generator delegation.

### Generators with Symbol.iterator
The above solution has one issue that makes the code look bad and difficult to handle. The iterator functions are completely different to the objects that represent our teams.

Here we will discuss the Symbol.iterator which will help us clean up our code by allowing us to merge our iterator directly into our engineeringTeam object so we won't have to ahead of time called teamIterator and pass in our team.

A simple definition is that Symbol.Iterator is a tool which teaches objects how to respond to the for of loop. Just like the yield* syntax we've seen previously, this will also be some strange syntax.

First let's refactor our testingTeam before doing engineeringTeam.

We have a testingTeam object and engineeringTeam object separately since it makes more sense but it would make even more sense if the testingTeam itself knew how to iterate over itself. In other words we don't want a separate function to exist just to iterate over the testingTeam. To solve this, we will add some code to the testingTeam object to tell it how to behave when somebody uses a for of loop on it.

Below we are adding to our testingTeam syntax using the Symbol.iterator and changed our teamIterator to `yield* team.testingTeam`. We also deleted the testingTeamIterator since we no longer require it.

```javascript {cmd="node"}
const testingTeam = {
  lead: 'Amanda',
  tester: 'Bill',
  [Symbol.iterator]: function* (){
    yield this.lead;
    yield this.tester;
  }
}

const engineeringTeam = {
  testingTeam,
  size: 3,
  department: 'Engineering',
  lead: 'Jill',
  manager: 'Alex',
  engineer: 'Dave'
};

function* teamIterator(team) {
  yield team.lead;
  yield team.manager;
  yield team.engineer;
  yield* team.testingTeam;
}

const names = [];
for (let name of teamIterator(engineeringTeam)) {
  names.push(name);
}

console.log(names);
```

An explanation of how this is working:
Firstly the teamIterator is doing the exact same thing as before. It yields the leader, manager and engineer but then we want to yield everything about the testingTeam. We are essentially telling the for of loop to do as best it can to iterate over the testingTeam. It will look in this object for a key of `[Symbol.iterator]` and if it can find that it will use the generator it is pointing at for iteration.
We are simply telling the for of loop how to iterate over the object.

The key is a completely valid ES6 key. This new key syntax is called `Key interpolation`. If we want to have a dynamically generated key, we can wrap is in `[]`. This does NOT form an array, it will be read as a completely normal key and the value of it is the generator function.

It is important to understand that now we have changed the iterator function within the `[Symbol.iterator]` key to use `this.` instead. This is because we're now sitting on the object and `this` becomes a reference to the testingTeam.

This has now cleaned up our code a lot. Now let's refactor the engineeringTeam to use the Symbol.iterator as well so we can get rid of our last big ugly standalone function which is just taking in a team. We will use the same pattern as before using `[Symbol.iterator]`.

Below we added another `[Symbol.iterator]` key to our engineeringTeam object and return using `this.` to refer to the properties on the engineeringTeam object. We also need to delegate using `yield*` as discussed previously to "drop" the iterator into the testingTeam object and search for the `[Symbol.iterator]` key in that object and iterate over it.

We can then delete the old iterator function and change our code to just `for (let name of engineeringTeam) {}`.

```javascript {cmd="node"}
const testingTeam = {
  lead: 'Amanda',
  tester: 'Bill',
  [Symbol.iterator]: function* (){
    yield this.lead;
    yield this.tester;
  }
}

const engineeringTeam = {
  testingTeam,
  size: 3,
  department: 'Engineering',
  lead: 'Jill',
  manager: 'Alex',
  engineer: 'Dave',
  [Symbol.iterator]: function*() {
    yield this.lead;
    yield this.manager;
    yield this.engineer;
    yield* this.testingTeam;
  }
};

const names = [];
for (let name of engineeringTeam) {
  names.push(name);
}

console.log(names);
```
As this is now getting quite complicated let's run through the code:
`[Symbol.iterator]` is a special object that's included in ES6 and it's job is to tell a `for of` loop how to iterate over an object (in this case, the `engineeringTeam` object). When a `for of` loop iterates over and object it will look to see if it has a defined `[Symbol.iterator]` property. Some objects such as arrays have this property set by default (which is why `for of` loop has no trouble iterating over an array). Arrays by default have this set up for you already but custom objects/iteration will need this defined for you.

Now when walking through the generator function, the generator will execute and pause on each `yield` statement. Each time we yield, this is considered a single iteration for our `for of` loop and the value of the variable declared in the `for of` loop becomes the value we yielded (In our case, the value `name` was declared to be this variable).

Finally, if the yielded value is another generator function, we can use `yield*` which tells the `for of` loop that it also needs to walk through any iterables in the generator function that the `yielf*` referenced. This will also attempt to iterate through the object by searching for *that* generator function for the `Symbol.iterator` property and iterate through that generator function yielding normally.

### Practical example
We now have a solid way to combine `for of` loops with iterators.

Now let's make a solution to iterate through a tree data structure. Trees are a popular data structure. Trees organise data in a series of nodes. There is a root node and from that each node on that tree can have children of those trees etc.

For our example we can imagine our tree structure to be the reddit comments system. In reddit, comments are shown in a tree structure where each new comment can have a child and each new child comment can have another child comment etc for infinity.

Now let's figure out how to create a tree and iterate through it.

We are using an ES6 class to construct each individual node.

Each node will have some content and a reference to the child (comment or array).

First let's set up our ES6 class. We want our nodes to contain the content and the children so let's set this up:

```javascript {cmd="node"}
class Comment {
  constructor(content, children) {
    this.content = content;
    this.children = children;
  }
}

const children = [
  new Comment('I thought it was good', []),
  new Comment('I thought your post was bad', []),
  new Comment('Meh', [])
];

const tree = new Comment('Let me know what you thought of my post', children);

console.log(tree);
```

Now we've created a root node we are calling `tree` and we've created an array of children when in turn could take another array of children etc.

Now we need to figure out how to iterate on this data structure.

Now, we could create a generator function which takes in a comment and call yield etc but we need to assume the depth will be of arbitrary depth. We need to ensure we **recurse** through the tree until the bottom level however many levels that may be.

To do that we need to figure out how to use the `[Symbol.iterator]` with some degree of iteration.

Previously we simply added `[Symbol.iterator]` to our object's property to tell the `for of` loop how to properly iterate. Now we are working inside of a Class instead, the syntax needs to be a little different if we want to use Symbol.iterator in a class.

This syntax is as follows:

```javascript {cmd="node"}
class Comment {
  constructor(content, children) {
    this.content = content;
    this.children = children;
  }

  *[Symbol.iterator]() {

  }
}

const children = [
  new Comment('I thought it was good', []),
  new Comment('I thought your post was bad', []),
  new Comment('Meh', [])
];

const tree = new Comment('Let me know what you thought of my post', children);

console.log(tree);
```
Notice the extra method we added to the Comment class after the constructor.
We lead with the * to indicate to Javascript we are going to write a generator.
Then we use the square brackets with Symbol.iterator which is how we call Symbol.iterator
Then we end with `()` as this is a method.

Inside of this generator we now need to yield some values for the `for of` iterator to understand how to iterate.

The way we plan to do it is simple:
For the top most node, we would just want to yield the content with `this.content`. Getting the current node data is not so difficult as a result.
The difficulty comes from recusing through all the children objects.
We can use the `yield*` syntax we used previously to tell the `for of` loop to "Fall through" to the generator it references.
We can use this to `Yield*` recusively down the tree until it has run out of things to iterate over and thus completing our iteration.

**NOTE:**
Although we are now focused on recursion which array helper methods are very good at doing, array methods do not work with generators so we cannot yield inside of an array helper. So whenever we want to iterate over some collection and yield every single value, we have to use the `for of` loop again.


```javascript {cmd="node"}
class Comment {
  constructor(content, children) {
    this.content = content;
    this.children = children;
  }

  *[Symbol.iterator]() {
    yield this.content;
    //Since array helpers do not work with generators we must use for of loop.

    //We want our for loop to go into every child it has and yield* in that child.
    for (let child of this.children) {
      yield* child;
    }
  }
}

const children = [
  new Comment('I thought it was good', []),
  new Comment('I thought your post was bad', []),
  new Comment('Meh', [])
];

const tree = new Comment('Let me know what you thought of my post', children);

const values = [];
for (let value of tree) {
  values.push(value);
}
console.log(values);
```

In the above code, we have now added recursion by saying in our iterator function within the class that for every child node of the current node (specified by the for of loop) we want to yield all back all the yields in those children and so fourth (specified by the yield*) until completely recursed.

Then below our class we are simply calling:

```javascript {cmd="node"}
const values = [];
for (let value of tree) {
  values.push(value);
}
console.log(values);
```
Which will simply be iterated through using the `[Symbol.iterator]` and those values stored in the array of `values`.
