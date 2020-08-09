# Chapter 2 - How the Internet works

This chapter covers:
- TCP/IP suite
- HTTP requests and responses
- Stateful connections
- Encryption

## The Internet Protocol Suite
Communication unreliable in the ARPANET days - TCP invented to make it more reliable.

TCP features:
- Data split into multiple segments
- Segments have a sequence number (so they can be put in order if later segments arrived first)
- Each segment requires receipt (so retransmission can take place if no receipt)
- Segments have checksums

## IP Adresses
Numbers assigned to individual computers (hosts). ICANN gives out parcels of adresses to ISPs, who
give them out to customers.

IPv4 address exhaustion (2^32 addresses aren't enough) - now IPv6 (2^128).

## The Domain Name System
Translates "domains" to IP addresses. Domain names must be registered with registrars.

Browsers look up new domains using a DNS server (ISP provided, or set by user/admin e.g. Google
DNS), and then *cached* to speed it up.
  - Amount of time this result is cached is defined by the TTL parameter on the DNS record
  - DNS poisoning is when this cache is modified so that certain domains point to IP addresses under
    the control of an attacker.

Domain name servers also store canonical name (CNAME) records, which are domain aliases, so multiple
domains can be pointed at the same IP address, i.e. the same server.
  - Not necessarily the same website! e.g. `en.example.com` and `de.example.com` can be used by the
    same server to differentiate requests for localised versions of a website.

Also can be used to route email using mail exchange (MX) records.

## Application Layer Protocols
Define how data should be interpreted once received.

Network, Internet and Transport layers are to get your data from A to B; Application layer is there
for the programs on the recipient to understand the data.

Examples: Simple Mail Transfer Protocol (SMTP), Extensible Messaging and Presence Protocol (XMPP,
for instant messaging), HTTP, SSH

## HyperText Transfer Protocol
HTTP connection:
- User agent (web browser) creates *requests* for certain resources
- Web server replies with *responses*.
  - Can have either the resource, or an error code

Both requests and responses are human readable text - can be compressed and/or encrypted, but not
necessary for HTTP.

An HTTP request contains:
- Method/verb - describes what the user agent wants the server to do (`PUT`, `GET`, `POST`, etc)
  - `GET` is the most common method. Retrieves a resource.
  - `POST` is for when information needs to be submitted to a server, e.g. for filling out forms.
    The information is sent in a `POST` request is in the request body.
    - `POST` must be used for submitting data to a server - using `GET` opens up CSRF attacks.
  - `PUT` uploads a resource, `DELETE` deletes. Usually triggered via JavaScript on the site.
  - `HEAD` is the same as a `GET`, but without the response body.
  - `CONNECT` starts a two-way communication - used for HTTP proxies
  - `OPTIONS` retrieves what methods the resource supports (can I `DELETE` it?)
  - `TRACE` instructs the server to reproduce the HTTP request they're responding to. Allows a
    client to determine if their request was altered on the way to the server.
    - `TRACE` is usually disabled on web servers because of security issues it raises (e.g. allows
      JavaScript injected into a page to access cookies that are deliberately inaccessible to
      JavaScript)
- URL - describes the resource being acted on
- Headers - for metadata (e.g. type of content, whether compression, what type of compression)
- (Optional) Body - any additional data that needs to be sent

An HTTP response contains:
- The status line, made up of:
  - Protocol description - `HTTP/1.1` or `HTTP/2`
  - Status code - 2xx (request accepted and responded), 3xx (redirect to different URL), 4xx (client
    error), 5xx (server error)
  - Status text - description of the status code (e.g. OK for 200, Not Found for 404)
- Headers - most responses have a `Content-Type` header which describes the resource (e.g.
  `text/html`, `image/png`)
  - There's also `Cache-Control`, which tell the client to cache the resource and for how long
- (Optional) Body - if it's `text/html`, this is where all the HTML is.
  - Other responses might be JavaScript, CSS, or binary data (images, sound, video etc)

## Stateful Connections
HTTP doesn't inherently distinguish between different user agents, because early on in the web all
pages were read only. I.e. HTTP was stateless.

Now we have dynamic pages that serve different content based on who is connecting (e.g. a shopping
cart), so we need to make HTTP stateful somehow. Stateful HTTP connections are called "HTTP
sessions".
  - A connection is stateful is when the connection is initiated by a handshake and when both
    parties keep sending data between each other until one decides to terminate the connection.

One way to establish an HTTP session is to create a cookie. Server's initial response includes a
`Set-Cookie` header, which the client will then include in any further requests in the same session.

Hackers would like to steal (or forge) cookies so they can impersonate other users. This is explored
in chapter 10.

## Encryption
HTTP is unencrypted. This opens up MITM attacks.

Modern HTTP connections are encrypted using Transport Layer Security (TLS). Protects confidentiality
and integrity. HTTP encrypted with TLS is known as HTTP Secure (HTTPS). To establish the symmetric
encryption key a TLS handshake must take place in the clear.

Encryption is explored in chapter 13.
