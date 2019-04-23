#Recursion
###What is Recursion?
Recursion is when a function calls itself until it completes it's task.

See the example below:
```javascript {cmd="node"}
let countDownFrom = (num) => {
  console.log(num);
  countDownFrom(num - 1);
}

countDownFrom(10);
```
If we run this, we see we get an error at the very bottom after our script ran for a bit into the minuses **Maximum call stack size exceeded**. The call stack is the stack of function calls that your code has made. In most non-functional programming languages there's an upper limit to how far you can go. Javascript DOES have this limitation in ES5 but this was removed in ES6 as this was one of the main limitations to functional programming.

In any case, this code is counting down way past 0 and the code does not work as expected as there is no stop condition. We can add an if to check the value like so:

```javascript {cmd="node"}
let countDownFrom = (num) => {
  if (num === 0) return;
  console.log(num);
  countDownFrom(num - 1);
}

countDownFrom(10);
```

This is a very simple example of recursion. It calls the function over and over until it does what it needs to do.

###Why use recursion?
In the above example we could have used a simple loop. This is true and everything a loop can do, recursion can do but this does not go the other way. There are some things recursion can do which loops cannot do.

```javascript {cmd="node"}
let categories = [
  { id: 'animals', 'parent': null },
  { id: 'mammals', 'parent': 'animals' },
  { id: 'cats', 'parent': 'mammals' },
  { id: 'dogs', 'parent': 'mammals' },
  { id: 'chihuahua', 'parent': 'dogs' },
  { id: 'labrador', 'parent': 'dogs' },
  { id: 'persian', 'parent': 'cats' },
  { id: 'siamese', 'parent': 'cats' }
]

//{
//  animals: {
//    mammals: {
//      dogs: {
//        chihuahua: null
//        labrador: null
//      },
//      cats: {
//        persian: null
//        siamese: null
//      }
//    }
//  }
//}
```

In the above example we can an output that looks like the commented tree structure.

```javascript {cmd="node"}
let categories = [
  { id: 'animals', 'parent': null },
  { id: 'mammals', 'parent': 'animals' },
  { id: 'cats', 'parent': 'mammals' },
  { id: 'dogs', 'parent': 'mammals' },
  { id: 'chihuahua', 'parent': 'dogs' },
  { id: 'labrador', 'parent': 'dogs' },
  { id: 'persian', 'parent': 'cats' },
  { id: 'siamese', 'parent': 'cats' }
]

let makeTree = (categories, parent) => {
  let node = {}
    categories
    .filter(c => c.parent === parent)
    .forEach(c => node[c.id] =
      makeTree(categories, c.id))

  return node
}

console.log(
  JSON.stringify(
    makeTree(categories, null)
    , null, 2)
)
```

We can see the above gives us a tree which is pretty much what we want.

To explain what just happened, when we call makeTree in out conosle.log we pass in categories. Categories is going to be all the categories and we will filter them all the ones which have the same parent.

For every such category we are going to forEach it and in the loop we will assign a property on the node with the same id as each category with the return value of ourselves but this time we are not passing in `null` as the parent parameter we are making a tree with the categories that have animals. They in turn will call makeTree and make trees where mammals are the parent category. They in turn will create a tree that contains just cats. This would go on and on but ends up running out of trees to make so ends it's execution.

To a degree, this could be possible with nested for loops but this will only work for a limted amount of loops as it becomes unmanageable very quickly if you need to start nesting 100 loops deep.
