#### Project objects
Just as proxies for HTTP will sit between the client and the server, often perfoming some sort of middle function such as mutation or logging of data, proxies can do the same in Javascript sitting between the client and a Javascript object.

As Javascript treats almost everything as an object, this means we can create proxies for any object including functions, arrays etc.

#### Syntax
Proxies have a constructor function which will take a `target object` and a `handler` as it's two parameters.

#### Handler
```javascript {cmd="node"}
const handler = {
	get: function() {
  	return 20;
  }
};

var prx = new Proxy({}, handler);

console.log(prx.test); //20
console.log(prx.b); //20
```
The above code creates a `get` handler and proxies on an empty object. Now any time an attempt is made to get a the value of an object, the number 20 is returned.
