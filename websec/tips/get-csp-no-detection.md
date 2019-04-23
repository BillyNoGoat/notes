### Get CSP details without triggering report-uri

Simply attempting to send data out from a site with CSP enabled can alert the site owner of the failed attempt if they specify a `report-uri`.
To avoid detection, we can get all the CSP data and read the headers:
```
fetch(document.location.href)
.then(resp => {
  const csp = resp.headers.get('Content-Security-Policy');
  // does this exist? Is is any good?
});
```
