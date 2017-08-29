---
layout: post
title: Luasec - Lua HTTPS Library
comments: True
---

I was working on the Luasec library over the summer mainly on fixing the HTTPS redirects, the CONNECT proxy implementation (for redirecting requests over the HTTP CONNECT tunnel) and adding support for HTTP/2(Client). 

My fork of Luasec(dev branch) can be found <a href="https://github.com/whoami-nr/luasec/tree/dev">here</a> which has all the recent updates as part of GSoC and all the relevant commits.

# Work done till now

<strong>HTTPS Module</strong>

I was working to add features for the HTTPS module during the first part of GSoC. It now supports the ability to talk HTTPS with a proxy, redirects through or without the proxy for HTTPS URLs, certain low level HTTP API functions are exposed and also supports SNI now. 


- CONNECT proxy support for HTTPS. Now Luasec can be used to initiate a CONNECT tunnel to a HTTP
proxy which enables the proxy to relay encrypted packets between Luasec and the final destination. This also works when redirects are enabled. While redirecting HTTPS->HTTPS or HTTP->HTTPS or HTTPS->HTTP(if the unsaferedirect paramter is set) it creates a new tunnel with the proxy for the new redirected destination. 

- Support for HTTPS redirects with an additional safeguard for preventing unsafe redirects from HTTPS->HTTP. This involves usage of the `unsaferedirect` paramter. 

- Merge and refactor the HTTP module from luasocket into luasec thus unifying the HTTPS module. All the HTTP low level functions have been imported into luasec now. This was done to increase code reuse within the library. 

- Test server name indication so that it conforms to section 3.1 of RFC 3546 which can found <a href="https://www.ietf.org/rfc/rfc3546.txt">here</a>.

More details on how to use it and references for the functions can be found on the wiki page of my fork <a href="https://github.com/whoami-nr/luasec/wiki/Luasec-HTTPS-Module">here</a>.

<strong>HTTP/2 Module</strong>

I went by implementing RFC's sequentially while also trying to make sure that I had a basic implementation for sending and receving frames working all along. 

The RFC I used can be found [here](http://httpwg.org/specs/rfc7540.html). 

The first two sections in the RFC are just an introduction and a generic protcol overview. 

1) Section 3

- It deals with starting a HTTP/2 connection. 

- Starting HTTP/2 for HTTP url's (No TLS) involved implementing a upgrade mechanism which informs the server of the upgrade request. For connections with TLS 

- The HTTP/2 connection module implementing ( send and receive frame functions) the support for sending basic frames like SETTINGS, HEADERS etc. 

- Add a HTTP -> HTTP/2 negotiation scheme so that upgrade requests can be sent from Luasec. 

- Maintain a Header table on the client side for implementation of HPACK later on. 

- Recieve a process a SETTINGS frame and then also send back a SETTINGS ACK frame thus establishing the stream parameters. 


One of the most important part of this was reading RFC's and learning to adhere to the specs. Also learnt a lot about debugging a network protocol implementation while getting to know the internals.


# Work to be done

- Remove the fake connection object from the HTTPS module which become pointless after the integration. 

- Make the connection module non blocking and add support for more frames. 

- Try to merge the existing PR supporting the ALPN negotiation scheme. 


# Roadmap for the HTTP/2 implementation and future work




