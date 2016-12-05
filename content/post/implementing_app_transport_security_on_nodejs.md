+++
date = "2015-10-01T13:48:22"
image = ""
math = true
tags = ["software", "nodejs"]
title = "Implementing App Transport Security on NodeJS"

+++

# What is App Transport Security? #
[ATS](https://developer.apple.com/library/prerelease/ios/technotes/App-Transport-Security-Technote/) is Apple's term for a set of "best practice" minimum security requirements that must be met in order for a client to establish a connection with a server. ATS is being enforced by default on `iOS 9`, and `OSX 10.11`.

On Stackoverflow respondents will tell you that `ATS = TLS 1.2`, but this is only partly true. ATS actually has 3 requirements:

- The server should support at least `TLS 1.2`
- The server should only support connection ciphers that provide `forward secrecy` (See the [complete list](https://developer.apple.com/library/prerelease/ios/technotes/App-Transport-Security-Technote/))
- Certificates needs to be signed with SHA-256, and use at least a 2048-bit RSA  key (or 256-bit ECC key)

# My experience implementing ATS #

## Diagnosing problems ##

Implementing ATS is actually pretty straight forward, but it took me a couple of days to get right. This was mostly due to my ignorance on network security. (In my defence I would note that the client gave supremely unhelpful error messages). Being able to diagnose the problem was the biggest challenge for me. Here are a few tools I found quite useful:

- On Mac OSX 10.11 (El Capitan) you can use `nscurl --ats-diagnostics https://example.com` to give you ATS diagnostic information.
- I also found <https://casecurity.ssllabs.com/> extremely useful.

## Implementation ##
Implementing ATS in Node is simple. A minimal implementation might look something like this:
```javascript
var https = require('https')
var fs = require('fs')

var privateKey = fs.readFileSync('/path/to/my/private/key', 'utf8')
var certificate = fs.readFileSync('/path/to/my/certificate', 'utf8')

var credentials = {
  key: privateKey,
  cert: certificate,
  ciphers: 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES128-SHA'
}

server = https.createServer(credentials, app)
```

## My mistakes ##

### Certificate chain ###
I am not sure if this was breaking ATS, but [casecurity.sslabs.com](https://casecurity.ssllabs.com/), reported that I was not providing the complete certificate chain, but only the client certificate.

I was using a certificate issued by [Godaddy](https://www.godaddy.com/), and I found the missing certificates in their [certificate repository](https://certs.godaddy.com/repository/). To include the certificate in my server, I followed [this](http://nginx.org/en/docs/http/configuring_https_servers.html#chains) tutorial. As a short summary:  You basically concatenate the two certificates together into a single file, which you use as your certificate file.

### Cipher mapping ###
You should only allow the list of accepted ciphers as described in the [ATS technotes](https://developer.apple.com/library/prerelease/ios/technotes/App-Transport-Security-Technote/#//apple_ref/doc/uid/TP40016240-CH1-SW3).

A caveat is that this list provides the `Cipher Suite Name`, but in the [Node  documentation](https://nodejs.org/api/tls.html#tls_modifying_the_default_tls_cipher_suite) they refer to the ciphers using their `OpenSSL names`. I found the mapping to convert between the names [here](http://openssl.org/docs/manmaster/apps/ciphers.html).

### Outdated NodeJS ###
Finally, and this took me the longest to resolve, I was using a very out of date version of `NodeJS`. My server was running [Ubuntu - Trusty Tahr](http://releases.ubuntu.com/trusty/), and I did a normal `apt-get install`, which installed `node v0.10.40`.

I do not know the minimum version for this configuration to work, but I would suggest installing the latest (`v4.1.1` at the time of writing) using the method described in the [node installation guide](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)
