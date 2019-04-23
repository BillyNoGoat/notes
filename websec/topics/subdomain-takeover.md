#### Sources:
1) https://www.hackerone.com/blog/Guide-Subdomain-Takeovers
2) https://www.slideshare.net/fransrosen/dns-hijacking-using-cloud-providers-no-verification-needed-76812183

### Info:
Subdomain takeover is where a particular subdomain may point to a specific external resource which is no longer available and could possibly be taken over. For example, github personal pages. See source 1 for info.

Another reason for subdomain takeover is if content is being loaded from another site. This could possibly even lead to an XSS attack if you could control the resource being loaded in. If being able to takeover a resource it may be useful checking if it's being loaded anywhere.

#### DNS Hijacking
(Source 2)
In some cases, when encountering dead DNS records do not just assume you cannot hijack the subdomain. `host` may return an error but `dig` could reveal the dead records.

#### Exploits
Once you have control of a subdomain, what can we do?
The first important step to take is to determine what the subdomain is being used for and attack it's usage.

##### Cookies
It is possible to modify cookies scoped to the root website. For example, `subdomain.example.com` can modify the cookies for `example.com`. This could be used to potentially hijack a victim's session on the base domain.

#### Cross-Origin Resource Sharing
CORS allows a host to share contents of a page cross-origin. Applications create a scope of rules which permit hosts to extract data INCLUDING authenticated data. Some apps may permit subdomains to make these sorts of HTTP requests with the assumption that the subdomains are trusted entities. After hijacking a subdomain, look for CORS headers (Burp suite's pro scanner usually picks them up)

### Non-cloud methods
#### Registration domain takeover
It's possible to takeover subdomains without the use of third party cloud providers such as AWS which is if the domain being used as a CNAME is up for registration. The attacker could simply register the domain in question if it available to be registered. The CNAME acts as an alias for whatever hostname it points to. If that hostname is a domain available for registration then you can take over the domain simply by registering it.

**NOTE** It is not sufficient to assume that if a DNS's zone file returns `NXDOMAIN` that it is available for registration. In some cases such as restricted top level domains (.gov, .mil etc) the domain may return `NXDOMAIN` despite not being available for registration.

##### NS Subdomain takeover
An issue with subdomain takeover is that normally the source domain name has multiple NS records. Multiple NS records are used for redundancy/load balancing reasons. If one of these Nameservers are able to be taken over, one of the two nameservers are chosen at random meaning an attacker may only be able to control 50% of the traffic on a given domain if they have control of one of the two nameservers.

In each scenario, things may play out differently:
- If a user is sent to a legitimate nameserver not in control by the attacker, the user will get the correct result returned and likely be cached for between 6 - 24 hours.
- If a user is sent to a nameserver in control by an attacker, they can provide a false result to a user which will also likely be cached. Since an attacker is in control of the nameserver, they are able to set the TTL to something like 1 week.

##### MX Subdomain takeover
MX Subdomain takeovers have the lowest impact compared to other takeovers as these are only often used for mail and as such can only affect emails addressed to the source domain name.
