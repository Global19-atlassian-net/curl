---
This is a quick-and-dirty proof-of-concept to add Token Binding support to `libcurl` and `curl`.

##### Prerequisites:
- have OpenSSL 1.1.x installed e.g. in `/usr/local/` and remove existing OpenSSL < 1.1.x libs/includes
  from `/opt/local` and/or `/usr/include` to avoid collusion
- checkout the `token_bind` source tree and create symbolic links in lib/vtls to the following files:
  - `token_bind_client.c`
  - `token_bind_common.c`
  - `cbs.c`
  - `cbb.c`
  - `base64.c`

##### Build
```
autoreconf --force --install && rm -rf autom4te.cache/
./configure --with-ssl=/usr/local --with-token-binding=<full_path_to_source>/token_bind
mqke && make install
```

##### Test
```
/usr/local/bin/curl -v https://www.zmartzone.eu:4433
```
Confirm that the `Sec-Token-Binding` header is present in the request and the `Provided-Token-Binding-Id` is in the JSON response (as a forwarded header).
```
$ /usr/local/bin/curl -v https://www.zmartzone.eu:4433
* Rebuilt URL to: https://www.zmartzone.eu:4433/
*   Trying 2001:470:78f6::1:1...
* TCP_NODELAY set
*   Trying 82.74.246.215...
* TCP_NODELAY set
* Connected to www.zmartzone.eu (82.74.246.215) port 4433 (#0)
* ALPN, offering http/1.1
* Cipher selection: ALL:!EXPORT:!EXPORT40:!EXPORT56:!aNULL:!LOW:!RC4:@STRENGTH
* successfully set certificate verify locations:
*   CAfile: /etc/ssl/cert.pem
  CApath: none
* TLSv1.2 (OUT), TLS handshake, Client hello (1):
* TLSv1.2 (IN), TLS handshake, Server hello (2):
* TLSv1.2 (IN), TLS handshake, Certificate (11):
* TLSv1.2 (IN), TLS handshake, Server key exchange (12):
* TLSv1.2 (IN), TLS handshake, Server finished (14):
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
* TLSv1.2 (OUT), TLS change cipher, Client hello (1):
* TLSv1.2 (OUT), TLS handshake, Finished (20):
* TLSv1.2 (IN), TLS handshake, Finished (20):
* SSL connection using TLSv1.2 / ECDHE-RSA-AES256-GCM-SHA384
*  # Connection negotiated token binding with key type: EC-DSA-P256
* ALPN, server accepted to use http/1.1
* Server certificate:
*  subject: CN=www.zmartzone.eu
*  start date: Apr 28 04:34:00 2017 GMT
*  expire date: Jul 27 04:34:00 2017 GMT
*  subjectAltName: host "www.zmartzone.eu" matched cert's "www.zmartzone.eu"
*  issuer: C=US; O=Let's Encrypt; CN=Let's Encrypt Authority X3
*  SSL certificate verify ok.
> GET / HTTP/1.1
> Host: www.zmartzone.eu:4433
> User-Agent: curl/7.55.0-DEV
> Accept: */*
> Sec-Token-Binding: AIkAAgBBQJe7TnJp7kp4TcH5Z4CkKWFsMew-kkv5lT8zBk1m8bKQpdIA5jC_e58IbHRIX3FYzP8H2cEU3OIDEwc9QTyLziMAQNBH5GNsKh3kL-KohbCHp7ov1VSQBvrYoa-2MdX0xWHLaiOGJ4rgjAZ13VHGpIlEAVdV1lvRn_1-xT0VZ_vixQ4AAA
> 
< HTTP/1.1 200 OK
< Date: Mon, 24 Jul 2017 14:30:05 GMT
< Server: meinheld/0.6.1
< Content-Type: application/json
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Credentials: true
< X-Powered-By: Flask
< X-Processed-Time: 0.000769853591919
< Content-Length: 444
< Via: 1.1 vegur
< 
{
  "headers": {
    "Accept": "*/*", 
    "Connection": "close", 
    "Host": "httpbin.org", 
    "Provided-Token-Binding-Id": "AgBBQJe7TnJp7kp4TcH5Z4CkKWFsMew-kkv5lT8zBk1m8bKQpdIA5jC_e58IbHRIX3FYzP8H2cEU3OIDEwc9QTyLziM", 
    "Token-Binding-Context": "AA0CEQqn602uM8WDsUFc8Hdsm2dpv1nnGa1lpvguUlPBbK8", 
    "User-Agent": "curl/7.55.0-DEV", 
    "X-Forwarded-Host": "www.zmartzone.eu:4433", 
    "X-Forwarded-Server": "www.zmartzone.eu"
  }
}
* Connection #0 to host www.zmartzone.eu left intact
```

---
![curl logo](https://cdn.rawgit.com/curl/curl-www/master/logo/curl-logo.svg)
[![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/63/badge)](https://bestpractices.coreinfrastructure.org/projects/63)
[![Coverity passed](https://scan.coverity.com/projects/curl/badge.svg)](https://scan.coverity.com/projects/curl)
[![Build Status](https://travis-ci.org/curl/curl.svg?branch=master)](https://travis-ci.org/curl/curl)
[![Coverage Status](https://coveralls.io/repos/github/curl/curl/badge.svg)](https://coveralls.io/github/curl/curl)

Curl is a command-line tool for transferring data specified with URL
syntax. Find out how to use curl by reading [the curl.1 man
page](https://curl.haxx.se/docs/manpage.html) or [the MANUAL
document](https://curl.haxx.se/docs/manual.html). Find out how to install Curl
by reading [the INSTALL document](https://curl.haxx.se/docs/install.html).

libcurl is the library curl is using to do its job. It is readily available to
be used by your software. Read [the libcurl.3 man
page](https://curl.haxx.se/libcurl/c/libcurl.html) to learn how!

You find answers to the most frequent questions we get in [the FAQ
document](https://curl.haxx.se/docs/faq.html).

Study [the COPYING file](https://curl.haxx.se/docs/copyright.html) for
distribution terms and similar. If you distribute curl binaries or other
binaries that involve libcurl, you might enjoy [the LICENSE-MIXING
document](https://curl.haxx.se/legal/licmix.html).

## Contact

If you have problems, questions, ideas or suggestions, please contact us by
posting to a suitable [mailing list](https://curl.haxx.se/mail/).

All contributors to the project are listed in [the THANKS
document](https://curl.haxx.se/docs/thanks.html).

## Website

Visit the [curl web site](https://curl.haxx.se/) for the latest news and
downloads.

## Git

To download the very latest source off the Git server do this:

    git clone https://github.com/curl/curl.git

(you'll get a directory named curl created, filled with the source code)

## Notice

Curl contains pieces of source code that is Copyright (c) 1998, 1999 Kungliga
Tekniska Högskolan. This notice is included here to comply with the
distribution terms.
