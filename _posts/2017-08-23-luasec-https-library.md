---
layout: post
title: Luasec - Lua HTTPS Library
comments: True
---

<strong>This page shall be continuously updated till August 29th. 
</strong>

I was working on the Luasec library over the summer mainly on fixing the HTTPS redirects, the CONNECT proxy implementation (for redirecting requests over the HTTP CONNECT tunnel) and adding support for HTTP/2 (both client on server).

My fork of Luasec(dev branch) can be found <a href="https://github.com/whoami-nr/luasec/tree/dev">here</a> which has all the recent updates as part of GSoC and all the relevant commits.

# Work done till now

<strong>HTTPS Module</strong>

- CONNECT proxy support for HTTPS. Now Luasec can be used to initiate a CONNECT tunnel to a HTTP
proxy which enables the proxy to relay encrypted packets between Luasec and the final destination. This also works when redirects are enabled. While redirecting HTTPS->HTTPS or HTTP->HTTPS or HTTPS->HTTP(if the unsaferedirect paramter is set) it creates a new tunnel with the proxy for the new redirected destination. 

- Support for HTTPS redirects with an additional safeguard for preventing unsafe redirects from HTTPS->HTTP. This involves usage of the `unsaferedirect` paramter. 

- Merge HTTP module from luasocket into luasec thus unifying the HTTPS module. All the HTTP low level functions have been imported into luasec now. This was done to increase code reuse within the library. 

- Test server name indication so that it conforms to section 3.1 of rfc 3546 which can found <a href="https://www.ietf.org/rfc/rfc3546.txt">here</a>.

More details and references can be found about this on the wiki page of my fork <a href="https://github.com/whoami-nr/luasec/wiki/Luasec-HTTPS-Module">here</a>.

<strong>HTTP/2 Module</strong>

- The HTTP/2 connection module implementing ( send and receive frame functions) the support for sending basic frames like SETTINGS, HEADERS etc. 

- Add a HTTP -> HTTP/2 negotiation scheme so that upgrade requests can be sent from Luasec. 

- Maintain a Header table on the client side for implementation of HPACK later on. 

- Recieve a process a SETTINGS frame and then also send back a SETTINGS ACK frame thus establishing the stream parameters. 


One of the most important part of this was reading a RFC's and learning to adhere to the specs. Also learnt a lot about debugging a network protocol implementation while 	getting to know the internals. 


# Work to be done

- Remove the fake connection object from the HTTPS module which become pointless after the integration. 

- Make the connection module non blocking and add support for more frames. 

- Try to merge the existing PR supporting the ALPN negotiation scheme. 




