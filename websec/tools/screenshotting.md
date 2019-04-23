### Sources
1) https://www.hackerone.com/blog/Guide-Subdomain-Takeovers
2) https://www.christophertruncer.com/eyewitness-usage-guide/

In some occasions it may be beneficial to take screenshots. For example in source 1, screenshots were being used to mass log how webpages on subdomains look at the time.

#### NOTE
It may be better to just log the webpages html content via a simple GET. This also allows for grepping of keywords. If not needed, continue.
##### Meg
Meg is a good tool for the above: https://github.com/tomnomnom/meg
`$ meg -d 10 -c 200 / live.txt`

#### EyeWitness
*source 2*
- https://github.com/FortyNorthSecurity/EyeWitness
This tool generates an HTML document containing all the screenshots, response bodies, and headers from your list of hosts.

#### Puppeteer/Headless chrome
Puppeteer can also easily be scripted to take mass screenshots with specific options such as viewport, browser agent etc.
