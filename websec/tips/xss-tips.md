Sources:
- https://www.youtube.com/watch?v=aybCeXhEjsA
- https://www.youtube.com/watch?v=PWMKVVLjXew

#### Escaping tricks
##### Execute function without braces
*Source 1*
You can execute javascript functions without braces by simply using backticks in their place.
Example:
`alert("test");`
This could be blocked by some sort of sanitiser. Instead we can use
```
alert`test`
```
to run the javascript.

#### Null bytes
*Source 1*
Often we can inject null bytes to do absolutely nothing but prevent parsers from properly reading our input. A null byte is: `%0a` in a url.

#### onerror
*Source 2*
Even if we are not able to embed `<script>` tags or code to be evaluated to Javascript, we may be able to embed another element and use an `onError` attribute to run javascript when an error is seen.

An example payload used against mopub.com is below:
`"><img src=x onerror=alert(1)>`
