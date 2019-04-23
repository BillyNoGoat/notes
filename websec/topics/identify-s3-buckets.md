**Sources**
1) https://medium.com/@localh0t/unveiling-amazon-s3-bucket-names-e1420ceaf4fa
2) https://labs.detectify.com/2017/07/13/a-deep-dive-into-aws-s3-access-controls-taking-full-control-over-your-assets/

### Identifying S3 buckets in AWS
**source 2**
An important thing to figure out is what s3 bucket a particular service uses. There are a number of ways to figure this out.

### Determine if you are on a bucket or not
**source 1**
https://labs.detectify.com/2017/07/13/a-deep-dive-into-aws-s3-access-controls-taking-full-control-over-your-assets/

### Getting the bucket name

#### URL structure/FQdN
In some instances we can make some educated guesses to what the name might be and use the url structure to make some educated guesses. Regardless, knowing the URL structure will help us test if we have the right bucket or not.

Often the FQDN (Fully qualified domain name) could be the s3 bucket name maybe with some characters substituted. For example, `images.somedomain.com` could be `images.somedomain.com.s3.amazonaws.com`.

From the `insecure-s3-buckets.md`, in topics we have the following url structures:
- `[bucketname].s3.amazonaws.com`
- `s3.amazonaws.com/[bucketname]`
- `[bucket-name].s3-us-west-2.amazonaws.com`
- `s3-us-west-2.amazonaws.com/[bucket-name]`
- `[bucket-name].storage.googleapis.com`
- `storage.googleapis.com/[bucket-name]`
- `http://[bucketname].s3-website-[region].amazonaws.com/`(if the bucket has the property “Static website hosting” you can browse to static html on this)

#### CNAME
A CNAME record basically acts as an alias for the subdomain so can be easily used to determine if a domain is using a bucket.

Below is an example of a real domain in a real report on hackerone after running a `dig` on the domain. It used the static website hosting option which adds `s3-website-[region].amazonaws.com` to the domain.
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

#### Server Header
Some example headers can often expose the bucket. Some amazon examples include:
- `x-amz-bucket-region: us-east-1`
- `x-amz-request-id: 8A1FG0417GE61`
- `x-amz-id-2: 8hAAqc4axNEt/TAXh0taX0eo27+l44=`

A google example:
- `X-GUploader-UploadID: AEnB2Up44Xul5VC__U57Uv_xrM5ZqQ`

#### Default page (Returned XML)
Often when simply accessing the bucket you can get some bucket information but also there may be a default `index` page with XML nodes such as:
- `<ListBucketResult xmlns="s3.amazonws.com/doc/2006-03-01">`
- `<Name>BucketNameHere</Name>`
- `<Contents>ContentDataHere</Contents>`

#### Error messages
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

#### Listing enabled
If listing objects `s3:ListBucket` is enabled, the name will be disclosed in the `<Name></Name>` element. This will be shown on the domain like `flaws.cloud.s3.amazonaws.com`

#### %C0 trick
A [tweet](https://twitter.com/0xmdv/status/1065581916437585920) exposed a trick to add `%C0` url encoded character at the end of an object UrL and the bucket name will be discused in the `<URI>` element on the page.
For example, `https://gov.domain.com/cs-ui/analytics/analytics.min.js%C0` will return a 400 error page with a URI containing the bucket name.
**NOTE**
This trick is limited as it will only work if the web server/application is internally redirecting/referencing the s3 bucket by URL path and the bucket should not be in the domain name for example:
`s3.amazonaws.com/<bucket_name>/<some_object>`
Instead of this, our web server/app is redirecting or referencing the full s3 endpoint name:
`<bucket_name>.s3.amazonaws.com/<some_object>`

The trick won't work, you will get a 400 error but the bucket name won't be discosed in the URI element.

#### Torrents
Amazon allows you to download files via a torrent client using BitTorrent.

Their documentation:

```
Retrieving a .torrent file for any publicly available object is easy. Simply add a “?torrent” query string parameter at the end of the REST GET request for the object. No authentication is required. Once you have a BitTorrent client installed, downloading an object using BitTorrent download might be as easy as opening this URL in your web browser.
```
The S3 bucket name is disclosed inside the `.torrent` file.

In order to get the torrent file, we need to do a couple of things:
Discover a publically available object in the S3 bucket (Brute-forcing, Google, Wayback Machine history, try to get creative here)

Add `?torrent` to the end of the object

Download the torrent file and parse it. There is a script for this: https://github.com/7sDream/torrent_parser

Prettify it

An example function `curl http://bucket.examplebucket.com/index.html\?torrent -s --output - | pytp | js-beatify`

The output will then be in nice json formatted output with a value in `info` of `x-amz-bucket`.

#### Bruteforcing
There is a ruby script called [lazys3](https://github.com/nahamsec/lazys3)  which will use patterns/permuatations based on a seed like a company name/subdomain and issues an HTTP request, gets the response header and shows them to you if it's not a 404.

**NOTE** be careful as we may interact with a bucket that is out of scope of the bounty.

#### Finding regions
[Here](https://docs.aws.amazon.com/general/latest/gr/rande.html) is a full list of all amazon regions for s3 buckets.

*TIP:*
*When interacting with an S3 endpoint in the form of <bucket_name>.s3.amazonaws.com or s3.amazonaws.com/<bucket_name>/(no region specified) we are in the us-east-1*
