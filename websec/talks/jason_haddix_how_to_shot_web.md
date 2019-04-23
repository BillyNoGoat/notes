## Discovery
#### Find a road less travelled
- You can attack the top application but it's probably been bashed and beaten a lot of the time already. It's probably had bounties for a long time. You want to find sub domains or obscure web servers on different ports.
- You may want to find acquisitions the company has made.
- Check for re-designs
- Check for mobile websites etc as they render differently etc.

### Tool: Recon-ng
Recon-ng is a tool which allows us to do muchh automated OSINT stuff including subdomain discovery and brute forcing. This is a big part of finding new surfaces to attack.
It will iteratively scrub google for websites on a given web property like \*.acme.com and remove those from the list until you have a long list of subdomains. It will also scrape from Bing, baidu and netcraft like a common fierce tool.
It's a wrapped around recgon-ng

### Further searching

We can perform a similar thing to recon-ng manually with google directly with something like `site: paypal.com -www.paypal.com -www.sandbox`. Here if you were to run with jusut the paypal.com you will find this `sandbox` subdomain so we can remove that from our search and go again (iterate) until no results are left as everything is filtered out in a list and we can then use that list for our advantage.

### Acquisitions
Acquisitions are common with large companies meaning a lot of possibly poor infrastructure can be taken on by larger companies. This information is often public information and accessable on wikipedia etc.

### Previous vulnerabilities
There's often links of previous vulnerabilities found for specific websites. If somebody else already found a bug, often these bugs are replicated in different places. Lots of times you can find the same bug in different locations. Can get a lot of intel about the applications which can help by allowing you to research how their infrastructure may be designed.

### Port scanning
Port scanning is not just for net pen, a full portscan of newly found targets will usually yield. Could help to locate separate web apps, extraneous services etc.
An example: `nmap -sS -A -PN -p --script=http-title dontscanme.bro` this will do syn scan, OS + Service fingerprint, no ping, all ports with HTTP titles.

## Mapping
Mapping is very important when doing bug bounties as you need to know your attack surface. Making notes on your attack surface is **crucial** to keep track as you go along.

### Tools
#### Google
Can get a lot of information from google and figure out what's going on there.
#### Smart directory bruteforcing
Finding unlinked content is also usueful. Lots of people use dirbuster and content discovery on burp etc.
The issue with these is that they're done by spidering the internet and using the results in highest priority to least to find common directories.
**RAFT** lists (included in seclists) are better for this type of work. They are lists that came out of an old application proxy. The lists lived on as it died and they are a spider of the internet's robots.txt files. It's a very useful list and often finds things.
**SVN digger** is the same as raft but for SVN projects. If the site is an open source place you can take all the paths and find config files etc. **Git digger** is the same thing for git.
#### Platform identification
- Wappalyzer (Chrome)
- Builtwith (Chrome)
Both above will analyse how the page is rendered, comments, common attributes etc to determine what a website was built with (the full stack).
- Retire.js
This will check the server side libraries and search for vulnerabilities for this.
- Check CVE's
Once you know the server's version numbers you can check for CVE's and attempt to exploit.

**For CMS's**
- WPScan (Wordpress)
This will help to locate plugins and users for a wordpress install and check for vulnerabilities for those.
- CMSMap for Drupal and Jumlo
Often WPScan etc can provide false positives but it's still very useful.

#### Directory bruteforce workflow
After bruteforcing directories, check for statusu codes indicating denial or require auth and append the list there to check for access control issues.
For example if the following provides you a 401 `www.acme.com/controlpanel` consider keep bruteforcing this directory such as `www.acme.com/controlpanel/[bruforce here]`.

#### Mapping/vuln discovery with OSINT
Find existing or previous issue:
- xssed.com
- Reddit XSS = /r/xss
- Punkspider (Burp engine which scans the internet)
- xss.cx
- xssposed.org
- twitter searching

This can give you an idea of the types of issues you can expect to find with these companies and could also help for regression testing on other parts of the company/other areas. Helps you know what you're up against. They're free and already out there.

#### Bugcrowdlabs maps
https://github.com/carnal0wnage/maps
bugcrowd created a bunch of json files which contains metadata for each bug bounty program out there. Over 250 such as minimum bounty and maximum and what mobile/web apps are included and what's excluded. Used all the data and fed it into lots of scripts and bruteforced for sub domains. Everybody can use it if they want to.

#### Intrigue tool
https://github.com/intrigueio/intrigue-core
A new tool to assist which does the following:
- DNS Subdomain Bruteforce
- Web spider
- Nmap scan
- Etc...

It's open source on github and useful for this

The possibilities aren't finite, it can do a lot of things.
