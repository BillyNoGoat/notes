Sources:
1) https://blog.securitybreached.org/2018/09/24/subdomain-takeover-via-unsecured-s3-bucket/
2) https://www.owasp.org/images/3/3d/DavidCalligaris-OWASPDay19-10-2018.pdf
3) https://github.com/EdOverflow/can-i-take-over-xyz

### Buckets
#### Overview
Buckets are services served by different cloud providers. Service providers include:
- Amazon S3
- Google storage
- DigtalOcean
- Spaces

Not all bucket services can be taken over by an attacker. A list of most services and their takeover methods can be found at **Source 3**.



#### aws cli
(Source 1)
Sometimes it may be possible to bypass normal rules for permissions by using the aws cli tool.

#### Identify A3 buckets:
We can make 100% sure we're reaching a s3 bucket by using the following 2 URLs:
- `[bucketname].s3.amazonaws.com`
- `s3.amazonaws.com/[bucketname]`
- `[bucket-name].s3-us-west-2.amazonaws.com`
- `s3-us-west-2.amazonaws.com/[bucket-name]`
- `[bucket-name].storage.googleapis.com`
- `storage.googleapis.com/[bucket-name]`

#### CDN / firewall
Often we can see buckets are kept behind CDN's or firewalls such as cloudfront (AWS's CDN solution) or Cloudflare. This can make it quite difficult to identify the actual bucket behind these barriers.

#### Identification
There are a few ways we can still identify buckets being used:

##### CNAME
A CNAME record basically acts as an alias for the subdomain so can be easily used to determine if a domain is using a bucket.

*Source 4*
Below is an example of a real domain in a real report on hackerone after running a `dig` on the domain:
```
michiel@msp ~ $ dig A a2.bime.io @8.8.8.8                                                                                                [2.1.8]

; <<>> DiG 9.9.5-11ubuntu1.2-Ubuntu <<>> A a2.bime.io @8.8.8.8
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 730
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;a2.bime.io.            IN  A

;; ANSWER SECTION:
a2.bime.io.     59  IN  CNAME   bimeio.s3-website-us-east-1.amazonaws.com.
bimeio.s3-website-us-east-1.amazonaws.com. 59 IN CNAME s3-website-us-east-1.amazonaws.com.
s3-website-us-east-1.amazonaws.com. 4 IN A  54.231.11.130
```

From this we can determine that the bucket is an Amazon S3 bucket located in the S3 US East 1 region and as this bucket was free to take, the reporter simply registered the domain.

##### Server Header
Some example headers can often expose the bucket. Some amazon examples include:
- `x-amz-bucket-region: us-east-1`
- `x-amz-request-id: 8A1FG0417GE61`
- `x-amz-id-2: 8hAAqc4axNEt/TAXh0taX0eo27+l44=`

A google example:
- `X-GUploader-UploadID: AEnB2Up44Xul5VC__U57Uv_xrM5ZqQ`

##### Default page (Returned XML)
Often when simply accessing the bucket you can get some bucket information but also there may be a default `index` page with XML nodes such as:
- `<ListBucketResult xmlns="s3.amazonws.com/doc/2006-03-01">`
- `<Name>BucketNameHere</Name>`
- `<Contents>ContentDataHere</Contents>`

##### Error messages
There may also be some error messages when visiting the bucket's URL.

Some examples include:
**NOTE** Below examples are only used for identification and does not imply vulnerable to attack.
```
404 Not found
- Code: NoSuchKey
- Message: The specified key does not exist.
- Key: index.html
- RequestId: REQUESTIDHERE
- HostId: HOSTIDHERE
```

```
404 Not Found
- Code: NoSuchWebsiteConfiguration
- Message: The specified bucket does not have a website configuration
```

```
<Error>
  <Code>AllAccessDisabled</Code>
  <Message>All access to this object has been disabled</Message>
  <RequestId>209B3AEF9974EC5D</RequestId>
  <HostId>Jpt3vmeNjQDfLcMQBYzTdlww2TgxsrcdVewfj91ktm4JX6Dr+OzjEVNTFTheEynpOVmgoUEUeFA=</HostId>
</Error>
```

```
<Error>
<Code>AccessDenied</Code>
<Message>Access Denied</Message>
<RequestId>6067FCD12E8D8FA0</RequestId>
<HostId>
IgwSxrsy6xBaBcoe1DcHVlojGPhRl9OwrqLgU+KBfyxQFwo1mgBKjGhngLT2LndxI2lLCv6wxX4=
</HostId>
</Error>
```

```
403 Forbidden
Code: AccessDenied
Message: Access Denied
RequestId: 53655F8ED81306B1
HostId: ztRJYxUnZbPEazQoFl4HBKHx/WpZrMzM4MmMQs0ZZkD5CJpyku3BWUlzGAKTe8EKxg2XWh40D7w=
```

#### Subdomain takeovers
`NoSuchBucket` response from Amazon AWS almost always results in domain takeover which is where the bucket is available for taking by an attacker by simply creating a new bucket where the company in question is pointing their domain to a bucket which is available.

#### Impact of subdomain takeovers
- Phishing
Subdomain takeovers can be used in phishing attacks as the domain a victim is accessing will seem legitimate as it will genuinely belong to the company they may be anticipating. As such, it may be impossible for a user to tell if the website is legitimate or not.
- Potentially steal scoped cookies with \*.example.com if the company in question decided to wildcard their domain.
- Potentially bypass CORS or CSP if the company in question implemented CSP for all subdomains of their domain
- Potential XSS attack if the subdomain in question was at one time being used to serve scripts and was forgotten about when the subdomain was removed.
