Sources:
1) https://github.com/sindresorhus/devtools-detect/issues/15#issuecomment-215908435

## Chrome Dev Tools
It is possible to detect if Chrome dev tools are open (even if undocked). This can be useful to prevent execution of certain tasks such as network requests if you know the user is going to inspect what is going on behind the scenes.

There's two ways of doing this (see source above):

```javascript {cmd="node"}
HTML:
status: <div id="devtool-status"></div>

Script:
var checkStatus;

var element = new Image();
// var element = document.createElement('any');
element.__defineGetter__('id', function() {
    checkStatus = 'on';
});

setInterval(function() {
    checkStatus = 'off';
    console.log(element);
    console.clear();
    document.querySelector('#devtool-status').innerHTML = checkStatus;
}, 1000)

```
And
```javascript {cmd="node"}
HTML:
status: <div id="devtool-status"></div>

Script:
var checkStatus;

var element = new Image();
// var element = document.createElement('any');
Object.defineProperty(element, 'id', {
  get:function() {
    checkStatus='on';
    throw	new Error("This is a hack to check If chrome Devtools is open. Pay no attention.............................................................................................................");
  }
});

setInterval(function() {
    checkStatus = 'off';
    console.dir(element);
    document.querySelector('#devtool-status').innerHTML = checkStatus;
}, 1000);


```
