*Sources*
- https://www.youtube.com/watch?v=lG7U3fuNw3A
#### Google XSS
Google's search box was recently vulerable to an XSS attack with the following URL:
`https://www.google.com/search?q=<noscript><p+title%3D"</noscript><img+src%3Dx+onerror%3Dalert(1)>">&cad=h`

#### Basic XSS avoiding
To avoid XSS attacks, it's advised that data is properly encoded based on the **context** of where the data will sit in your webpage.

Data needs to be encoded differently if your data is to sit within the DOM/`<script>`/HTML Attribute/SVG, etc.

Nowadays, this is normally handled for you when using large web frameworks.

However, there's still a problem with sanitising HTML.

#### Client sided XSS prevention
Most of the time, we want XSS to be validated on the server side. To push the data to the server, let the server parse it as it sees fit and return an error if it wasn't
