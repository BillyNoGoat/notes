#### Sources:
1) https://www.hackerone.com/blog/Guide-Subdomain-Takeovers
2) https://pentester.io/commonspeak-bigquery-wordlists/

### Subdomain scraping
We must first differentiate between scraping and brute forcing, as both of these processes can help you discover subdomains, but can have different results. Scraping is a passive reconnaissance technique whereby one uses external services and sources to gather subdomains belonging to a specific host. Some services, such as DNS Dumpster and VirusTotal, index subdomains that have been crawled in the past allowing you to collect and sort the results quickly without much effort.

#### Sublist3r
https://github.com/aboul3la/Sublist3r
Simplest subdomain scraping tool. Python script to gather subdomains from numerous search engines, SSL certs and websites like DNS Dumpster
```
$ git clone https://github.com/aboul3la/Sublist3r.git
$ cd Sublist3r
$ sudo pip install -r requirements.txt
```

### Bruteforcing
Bruteforcing is where you use a wordlist based on the popularity of the subdomain names to try to find subdomains
*Note:* Ensure wildcards are off to avoid false positives. Easily tested with a `long random string.subdomain` like `idjosajd93849y98hdfs.example.com` as it's not going to exist.

Jason Haddix has created a wordlist of every DNS enumeration tool out there:
https://gist.github.com/jhaddix/86a06c5dc309d08580a018c66354a056

### Tools

#### Amass
https://github.com/OWASP/Amass
Written in Go by OWASP. Uses a mix of web scraping, bruteforcing, archived data sources, permuting names and reverse dns lookups to find subdomains.
Usage example (docker):
`docker run amass --passive -d discordapp.net`

#### Altdns
https://github.com/infosec-au/altdns
Used to recursively bruteforce subdomains and provide custom wordlists after fingerprinting.

#### Commonspeak
https://github.com/pentester-io/commonspeak
Generate word lists based on Google's BigQuery to identify current trends. Allows you to stay much more up to date with what's relevant. See source 2 for more information.

#### Subfinder
https://github.com/subfinder/subfinder
Combines both scraping and brute forcing. General purpose and popular subdomain discovery tool. For more results, include API keys.

#### Massdns
https://github.com/blechschmidt/massdns
MassDNS focuses on speed for subdomain enumeration. You should provide it with a list of valid resolvers, your resolvers should be kept up to date and you should check to see which ones return best results. See https://public-dns.info/nameservers.txt for more info.

`$ ./scripts/subbrute.py lists/names.txt example.com | ./bin/massdns -r lists/resolvers.txt -t A -o S -w results.txt`

### Documentation
Some useful websites to aid with subdomain takeover are:
- https://github.com/EdOverflow/can-i-take-over-xyz
A git page outlining how to identify each subdomain takeover possibility with information about completing them.
- https://dzone.com/articles/what-are-subdomain-takeovers-how-to-test-and-avoid
