Sources
1) https://github.com/nccgroup/autochrome

### Autochrome
Autochrome is a version of Chromium which can help to distinguish HTTP traffic when using a tool such as burpsuite. It also has some other features which makes pentesting much easier:
- Turning off auto-updaters
- No XSS auditor
- Automatic proxy configuration (uses 127.0.0.1:8080)
- Doesn't check TLS Certs
- Basic integration with Burp if you install the included Burp extension

Autochrome also comes with several small utility extensions; you can add more in the data/extensions directory.
