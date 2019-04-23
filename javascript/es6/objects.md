### Objects

Javascript doesn't have object inheritance, it instead uses a system called prototypal inheritance.

ES6 has a solution to a messy system in Javascript. We can use classes in ES6 to set up inheritance and create objects.

**NOTE:**
Classes are not an absolute solution. Under the hood, we are still using prototypal inheritance.

Here we will create a car object which will take a `title` as a property.
ES5 (constructor function):
```javascript {cmd="node"}
function Car(options) {
  this.title = options.title;
}

const car = new Car({ title: 'Focus' });

console.log(car);
```

Above is a simple way of creating an object using a constructor function in ES5. Now we want to add some methods to our car.

Whenever we want to add a new method to a class in Javascript, we add it to the prototype object of the constructor:

```javascript {cmd="node"}
function Car(options) {
  this.title = options.title;
}

Car.prototype.drive = function() {
  return 'vroom';
}

const car = new Car({ title: 'Focus' });

console.log(car.drive());
```
This is basic object creation in Javascript. We create a new class with a constructor function, we create a new object of that class with the `new` keyword and if we want to add new methods on it we add it on the prototype property of the constructor.

This is a difficult thing to explain and in the ES6 class system we do not need to worry about the `prototype` object anymore.

### ES6 class

Firstly we will stick with our orignal ES5 class system to add an object which inherits from Car and then we will refactor it to use ES6 classes.

So here we need to make a new object. Let's create a Toyota class and create a new toyota object.
Now we need to link the Toyota object to inherit from the Car class.

Firstly, we need to ensure when we call the Toyota constructor we run any initialization that occurs in the Car object aswell. We can do this by doing `Car.call(this, options);`.

We also need to ensure we can call the Car's `drive` method from a Toyota object.
```javascript {cmd="node"}
function Car(options) {
  this.title = options.title;
}

Car.prototype.drive = function() {
  return 'vroom';
}

function Toyota(options) {
  this.colour = options.colour;
}

Toyota.prototype = Object.create(Car.prototype);
Toyota.prototype.constructor = Toyota;

Toyota.prototype.honk = function() {
  return 'beep';
}

const toyota = new Toyota({ colour: 'red', title: 'Work car'});

console.log(toyota);

console.log(toyota.drive());
console.log(toyota.honk());
```

As we can see this is a very complicated solution that is pretty overwhelming. Classes are created to solve this issue and simplify the process required to create object oriented code.

#### Writing in ES6

Classes in ES6 have a slightly different syntax in Javascript to anything else.
We can use the `class` keyword to create a new class in Javascript. We can then create this using the same `new` keyword we normally do.

To create a method in a class, we use the enhanced object literal syntax. The whole idea of this syntax is that whenver we want to write a function we no longer need to use `function` etc and set a function. We can just add `drive()` directly to the object literal.

```javascript {cmd="node"}
class Car {
  drive() {
    return 'vroom';
  }
}

const car = new Car();
console.log(car.drive());
```

Now one thing is still missing here. We can add a method but we still need a place to set the title property on the car. Previously we had a constructor function.

In ES5 when we called the `new` keyword we called the constructor function for `Car` and do some initialization/setup for that object. Now we need a similar setup here in ES6. This setup is going to be a specific method we need to add to our object called `constructor()`. You can think of it as `init` or `create` etc. It will run whenever we create a new object of this class.

So just as we did in ES5 let's create the constructor to take an options parameter with this new syntax and create a new car object with our options:

```javascript {cmd="node"}
class Car {
  constructor(options) {
    this.title = options.title;
  }

  drive() {
    return 'vroom';
  }
}

const car = new Car({ title: 'Toyota' });
console.log(car.drive());
```

**NOTE:**
We can actually do some refactoring to this and make our `options` destructured:


```javascript {cmd="node"}
class Car {
  constructor({ title }) {
    this.title = title;
  }

  drive() {
    return 'vroom';
  }
}

const car = new Car({ title: 'Toyota' });
console.log(car);
```

**NOTE:**
When adding class methods, we used the enhanced object literal syntax of just `drive() {}` etc and we do not need to separate these with commas.

Now we have something very similar to ES5 but we don't have any reference to Prototype etc.

#### Inheriting from another class

As we did before, we want to extend from the Car class.

Remember the aim is to get all the configuration/setup that Car has (the constructor function needs to be run on our Toyota object) and also we need access to all of it's methods. In addition we want to ensure Toyota has it's own constructor setup.

```javascript {cmd="node"}
class Car {
  constructor({ title }) {
    this.title = title;
  }

  drive() {
    return 'vroom';
  }
}

class Toyota {
  constructor({ colour }){
    this.colour = colour;
  }

  honk(){
    return 'beep';
  }
}

const toyota = new Toyota({ colour: 'red', title: 'Work car'});
console.log(toyota);
```

At this point we have both classes set up with constructors and methods added to them and can create a new Toyota object. Now all we need to add is the ability to get all the methods coming from the Car class and do the initialization that Car does (It's constructor).

The syntax for this does not care about the `prototype` object. Instead the syntax is simply to add `extends Car` to the end of our new class creation:

```javascript {cmd="node"}
class Car {
  constructor({ title }) {
    this.title = title;
  }

  drive() {
    return 'vroom';
  }
}

class Toyota extends Car {
  constructor({ colour }){
    this.colour = colour;
  }

  honk(){
    return 'beep';
  }
}

const toyota = new Toyota({ colour: 'red', title: 'Work car'});
console.log(toyota.drive());
```

When we use the extends keyword we are saying, "I want Toyota to have access to all the methods and setup inside of Car.". We also need to know that when we create a new Toyota we want to run the Car's constructor too as part of the initilization, for example in this case we want to ensure our object gets a `title` property which is done in the Car class's constructor function.

The syntax for this is to call `super();` inside the Toyota constructor method. This is evident by the error message provided when running the above block of code which results in `Must call super constructor in derived class before accessing 'this' or returning from derived constructor` .

An explanation for this is as follows:
- Both Toyota and Car have a constructor method but we want to call the Car's constructor method too.
- Whenever we have a subclass which wants to call a method on the parent, we refer to the `super();` keyword.
- We can think of the `super()` keyword as being `Car.constructor()`.

As another example, if both the Car and Toyota had a method called `honk()` (Much like how they both have a constructor method) we could call `super()` in the subclass's honk method to ensure it would run the parent class's `honk()` method aswell.

One last thing we need to do to solve our issue is that just adding `super();` calls the Car constructor which expects options containing the `{ title }` which we aren't providing. When we call the super we need to include the options that this constructor expects.

This makes our destructuring more of a pain to handle so we'll switch back the destructuring for the child class and pass the whole options object to the parent class which will destructure the `{ title }`.

```javascript {cmd="node"}
class Car {
  constructor({ title }) {
    this.title = title;
  }

  drive() {
    return 'vroom';
  }
}

class Toyota extends Car {
  constructor(options){
    super(options);
    this.colour = options.colour;
  }

  honk(){
    return 'beep';
  }
}

const toyota = new Toyota({ colour: 'red', title: 'Work car'});
console.log(toyota.drive());
```

Now we have the complete class setup and I can now access the drive method on the Toyota object inherited from the parent.

### When to use classes?

In practice, Javascript's community has made a huge embrace of classes but some ES6 features were just nice editions which made things easier and could be forgotten. Now with the Class and Extends keywords, using Classes are very useful.
