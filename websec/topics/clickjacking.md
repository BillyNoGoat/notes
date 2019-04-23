Sources:
1) https://blog.innerht.ml/google-yolo/
2) https://lightningsecurity.io/blog/linkedin/
3) https://github.com/cure53/browser-sec-whitepaper/blob/master/browser-security-whitepaper.pdf
4) https://hackerone.com/redirect?signature=0ca757f1a5e12a2638f23409eb52815b2f659f38&url=https%3A%2F%2Fbugzilla.mozilla.org%2Fshow_bug.cgi%3Fid%3D725490

#### Clickjacking
Clickjacking allows you to place your own clickable DOM elements over other elements in DOM. This is normally enabled using CSS trickery.

#### CSS that makes this work
- `position` is a CSS property that allows an element to sit on top of another element. The `pointer-events` CSS property allows events to passtrhough an element so the click actually registers with the element underneath. Combining these allows you to fake a button on top of the button the user intends to click.

- `opacity` can also be used for this to ensure the button you want people to click is over opaque and sits over the top of the button the user thinks they are clicking on. This also doesn't rely on `pointer-events` as you now click the top most button.

#### Use cases
One example would be to hide a button over the top of a facebook like button embed. The facebook button embed allows developers to have a way for users to like a webpage in a single click. A single click is all we need so we can easily clickjack for facebook likes.

##### LinkedIn
(source 2)

### Defences
Some defences against clickjacking can be circumvented.
- `X-Frame-Options: SAMEORIGIN`
This option will only check if the embedded frame is on the same origin of the top window.
For example, an attacker can do something like twitter.com > attacker.com > twitter.com to evade detection. See source 4 for more details.

- `Content-Security-Policy: frame-ancestors 'self'`
This is the only correct way to prevent framing from other websites (It will check agains the ancestor list). This is effective but this is for CSP2 which not all browsers support. Safari and IE are still vulnerable.

- JS-based frame-buster
Some pages may contain javascript to prevent frames from being embedded. This is easily circumvented using the `sandbox` attribute which disables Javascript of a frame.
