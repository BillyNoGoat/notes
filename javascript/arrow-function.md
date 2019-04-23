###Arrow functions

Arrow functions are functions which are much shorter versions of regular functions which allows us to program differently by using functions in-line with the rest of our code.

Function arrows are ideally used in-line as one-time use functions as arrow functions cannot be named.

###Implicit return
Due to these functions being used in-line, arrow functions which have only one statement can be used to return the value implicity. Because these are just single statements, we need just one line and the word `return` is not required.

###Managing parameters
If you have just one parameter, you can completely ommit the parenthesis from your code (Seen in the refactored code where `filter` parameter `event` is not in it's own parenthesis).

However, when using multiple parameters, they will need to be wrapped correctly in parenthesis (Seen in the refactored code where `reduce` parameters `prev` and `value` and passed in).

###Example:
```javascript {cmd="node"}
const dragonEvents = [
  { type: 'attack', value: 12, target: 'player-dorkman'},
  { type: 'yawn', value: 40},
  { type: 'eat', target: 'horse'},
  { type: 'attack', value: 23, target: 'player-fluffykins'},
  { type: 'attack', value: 12, target: 'player-dorkman'}
]

const totalDamageOnDorkman = dragonEvents
  .filter(function(event) {
    return event.type === 'attack'
  })
  .filter(function (event){
    return event.target === 'player-dorkman'
  })
  .map(function (event){
    return event.value
  })
  .reduce(function(prev, value) {
    return (prev || 0) + value
  })

console.log(totalDamageOnDorkman);
```
###Refactored example:
This can be re-factored into the following:
```javascript {cmd="node"}
const dragonEvents = [
  { type: 'attack', value: 12, target: 'player-dorkman'},
  { type: 'yawn', value: 40},
  { type: 'eat', target: 'horse'},
  { type: 'attack', value: 23, target: 'player-fluffykins'},
  { type: 'attack', value: 12, target: 'player-dorkman'}
]

const totalDamageOnDorkman = dragonEvents
  .filter(event => event.type === 'attack')
  .filter(event => event.target === 'player-dorkman')
  .map(event => event.value)
  .reduce((prev, value) => (prev || 0) + value)

console.log(totalDamageOnDorkman);
```

So we can note that in the line `.filter(event => event.type === 'attack')`, `event` is simply our function parameter, and the result of statement `event.type === 'attack'})` will automatically return without requiring a `return` keyword.
