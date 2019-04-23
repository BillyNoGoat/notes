### Features of React

#### Declarative
It's state based. This means we declare what state a component is in and React decides how to render that.

#### Component-based
We can create self contained components which can be dropped into other components and these are contained so our code stays contained.

#### Unopinionated
It's just a view layer so you can incrementally put it into your project without having to replace your entire codebase.

#### Virtual Dom
React uses something called the Virtual Dom. It will look at your code before and after changes and only change the minimum amount to make those changes exit in the DOM. Also, the virtual DOM allows us to not directly manipulate the DOM and instead change the virtual DOM. This is much better for performance.

### Creating a react app
Facebook have created a node module to easily create a react app:
https://github.com/facebook/create-react-app

To use this we can follow the instructions:
`NPX create-react-app my-app` (Need NPM 5.2 or higher)
We can then cd into the new directory as instructed and type `npm start`. Our react app should open with a boilerplate "Welcome to react" page and we're ready to get started.

### Generated files
#### index.js
In index.js we can see:
```javascript {cmd="node"}
ReactDOM.render(<App />, document.getElementById('root'));
registerServiceWorker();
```

We are rendering the component `<App />` onto the element `root` which we can find in the `index.html` file. This is being rendered in the root. We can Ctrl + click on App in this file to get to the `App.js` file.

#### App.js
React is normally written in ES6 Syntax hence the `class` and `import` keywords in this file.

We can see we are creating a new class which extends `component`. The `component` comes from react as we can see in the first line, this class is basically the heart of all components.

Inside of this component we have a `render()` method which returns the element/HTML we are going to render in our app.

We can add onto our components new methods such as `onClick(){}` just as we would add methods to any ES6 class. We will go into this further later.

React is written in JSX syntax. Notice we have HTML elements within our Javascript which is unusual. This is compiled using something external, this is done under the hood by a babel compiler hidden in react-scripts which we can see in package.json.

We COULD re-write our components in pure Javascript but it's not as clean or intuitive. There are some slight differences with JSX Syntax though such as divs using `className` instead of `class`.
Another example is that we may need to use an HTML attribute `htmlFor` instead of just `for`. The console will give warnings if mistakes are made.

##### Render function
This is the foundation of React components as it decides what gets rendered on the page. With JSX syntax we can write the Javascript into the HTML such as creating variables.
Below we will edit the render function to show this:

```javascript {cmd="node"}
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    const title = 'This is Billy\'s app';
    return (
      <div className="App">
        <h1>{title}</h1>
      </div>
    );
  }
}

export default App;
```
Note the variable in React uses the syntax `{varName}`.
We can also write logic into this like below:

```javascript {cmd="node"}
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    const title = 'This is Billy\'s app';
    const secondTitle = 'This is NOT Billy\'s app';
    return (
      <div className="App">
        <h1>
        {
          true ? secondTitle : title
        }
          </h1>
      </div>
    );
  }
}

export default App;
```
See here we used a Ternary operator to perform some logic. Generally speaking we want to avoid this as much as possible with our React app and instead want to write them into helper functions above the render function.

We can also generate lists of data using logic. Here we will create an array and print a list of objects:

```javascript {cmd="node"}
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    const list = [
      'Item 1',
      'Item 2',
      'Another Item'
    ]
    return (
      <div className="App">
        <h1>
        {
          list.map(function(item){
            return item;
          })
        }
          </h1>
      </div>
    );
  }
}

export default App;
```

Now we can see from the above example, this does not create each object with new lines so we can now edit our logic to include divs:

```javascript {cmd="node"}
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    const list = [
      'Item 1',
      'Item 2',
      'Another Item'
    ]
    return (
      <div className="App">
        <h1>
        {
          list.map(function(item){
            return (
              <div>{item}</div>
            )
          })
        }
          </h1>
      </div>
    );
  }
}

export default App;
```

#### Event handling in JSX
To add an event handler to the above code, we can just add them as HTML attributes such as `onClick` or `onMouseEnter` etc. Let's add an onClick handler to the divs we are generating:

```javascript {cmd="node"}
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  onClick() {
    alert("Clicked");
  }

  render() {
    const list = [
      'Item 1',
      'Item 2',
      'Another Item'
    ]
    return (
      <div className="App">
        <h1>
        {
          list.map(item => {
            return (
              <div onClick={this.onClick}>{item}</div>
            );
          })
        }
          </h1>
      </div>
    );
  }
}

export default App;
```

Here we have added the attribute `onClick` to our div HTML element and also added a method of `onClick` to our class which gets called with `this.onClick`. The html attribute needs to have the `onClick` but the method name could be called anything and called using `this.anyMethodName()`.

##### Further event handing
Now let's add an `onChange` event for an input box. Here we add an input in the bottom of our HTML

```javascript {cmd="node"}
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  onChange(event) {
    console.log(event.target.value);
  }

  onClick() {
    alert("Clicked");
  }

  render() {
    const list = [
      'Item 1',
      'Item 2',
      'Another Item'
    ]
    return (
      <div className="App">
        <h1>
        {
          list.map(item => {
            return (
              <div onClick={this.onClick}>{item}</div>
            );
          })
        }
          </h1>
          <input onChange={this.onChange} />
      </div>
    );
  }
}

export default App;
```

We can also add an `onSubmit` listener for a form like so:

```javascript {cmd="node"}
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  onSubmit(event) {
    event.preventDefault();
    alert('submitted');
  }

  render() {
    const list = [
      'Item 1',
      'Item 2',
      'Another Item'
    ]
    return (
      <div className="App">
        <h1>
        {
          list.map(item => {
            return (
              <div>{item}</div>
            );
          })
        }
          </h1>
          <form onSubmit={this.onSubmit}>
            <input />
          </form>
      </div>
    );
  }
}

export default App;
```
We can test this submit app by simply hittin the enter key whilst in the input contained for the form.

##### Unique Key prop
**NOTE:**
We may note at this point that the console shows us a warning `Each child in an array or iterator should have a unique "key" prop.`.
Whenever we render an array we want to render a unique key prop. Here we can simply add `key={item}` to our div to fix this error.

### Refs
If we add a ref attribute to an element, it will be a simple reference to that element.
In the above code, we can do add an attribute `ref={input = > this.input = input} />`.
Here we are making this ref a function where input is the parameter and we are setting this.input equal to the entire input element.
