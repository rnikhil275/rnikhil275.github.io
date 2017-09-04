---
layout: post
title: Luasec - Lua HTTPS Library
comments: True
---

I was working on the Luasec library over the summer mainly on fixing the HTTPS redirects, the CONNECT proxy implementation (for redirecting requests over the HTTP CONNECT tunnel) and adding support for HTTP/2(Client). 

My fork of Luasec(dev branch) can be found <a href="https://github.com/whoami-nr/luasec/tree/dev">here</a> which has all the recent updates as part of GSoC and all the relevant commits.

# Work done till now

<strong>HTTPS Module</strong>

I was working to add features for the HTTPS module during the first part of GSoC. It now supports the ability to talk HTTPS with a proxy, redirects through or without the proxy for HTTPS URLs, certain low level HTTP API functions are exposed and also supports SNI now. Work done in this section are relevant to this [file](https://github.com/whoami-nr/luasec/blob/dev/src/https.lua).


- CONNECT proxy support for HTTPS. Now Luasec can be used to initiate a CONNECT tunnel to a HTTP
proxy which enables the proxy to relay encrypted packets between Luasec and the final destination. This also works when redirects are enabled. While redirecting HTTPS->HTTPS or HTTP->HTTPS or HTTPS->HTTP(if the unsaferedirect paramter is set) it creates a new tunnel with the proxy for the new redirected destination. 

- Support for HTTPS redirects with an additional safeguard for preventing unsafe redirects from HTTPS->HTTP. This involves usage of the `unsaferedirect` paramter. 

- Merge and refactor the HTTP module from luasocket into luasec thus unifying the HTTPS module. All the HTTP low level functions have been imported into luasec now. This was done to increase code reuse within the library. 

- Test server name indication so that it conforms to section 3.1 of RFC 3546 which can found <a href="https://www.ietf.org/rfc/rfc3546.txt">here</a>.

More details on how to use it and references for the functions can be found on the wiki page of my fork <a href="https://github.com/whoami-nr/luasec/wiki/Luasec-HTTPS-Module">here</a>.

<strong>HTTP/2 Module</strong>

This portion of the module was worked on during the second part of GSoC. I went by implementing RFC's sequentially while also trying to make sure that I had a basic implementation for sending and receving frames working all along. Work done in this section are relevant to these files. 

[1) Error Module](https://github.com/whoami-nr/luasec/blob/dev/src/http2_error.lua)

[2) Stream Module](https://github.com/whoami-nr/luasec/blob/dev/src/http2_stream.lua)

[3) Codec Module](https://github.com/whoami-nr/luasec/blob/dev/src/codec.lua)

[4) Bit Operation Module](https://github.com/whoami-nr/luasec/blob/dev/src/bit.lua)

The Codec Module is used for string packing and unpacking. It provides a unified interface for various modes in `string.pack` and `string.unpack` functions. The Bit operation module smoothens out all the various lua bit libraries and versions. The `bit` libary with luajit, `bit32` libary with lua 5.2 and lua 5.3 built in bit operators are wrapped in a unified function. 

The RFC I used can be found [here](http://httpwg.org/specs/rfc7540.html). I also used the lua-http module for lot of reference code. The module can be found [here](https://github.com/daurnimator/lua-http/)

The first two sections in the RFC are just an introduction and a generic protcol overview. 

1) [Section 3](http://httpwg.org/specs/rfc7540.html#rfc.section.3)

- It deals with starting a HTTP/2 connection. 

- Starting HTTP/2 for HTTP url's (No TLS) involved implementing a upgrade mechanism which informs the server of the upgrade request. For TLS connections the socket is just wrapped with luasec. 

- The connection preface is sent after both the client and server have decided to use HTTP/2.

- This section has been completely implemented. 

2) [Section 4](http://httpwg.org/specs/rfc7540.html#rfc.section.4)

- It deals with support for all the relevant frame types.

- The HTTP/2 connection module implementing ( send and receive frame functions) the support for sending basic frames like SETTINGS, HEADERS etc has been finished. It supports 

- Add a HTTP -> HTTP/2 negotiation scheme so that upgrade requests can be sent from Luasec. 

- Maintain a Header table on the client side for implementation of HPACK later on. 

- Recieve a process a SETTINGS frame and then also send back a SETTINGS ACK frame thus establishing the stream parameters.

- Implemented functions for writing priority(which specifies the sender advised priority of the stream), rst_stream (which allows for immediate termination of stream), ping(which helps measure the minimal roundtrip from the sender as well as for determining whether an idle connection is functional), data(which sends http data), headers(which sends http headers), window_update frames(which is used for implementing flow control), settings( stream session settings), push_promise(which notifies the peer endpoint in advance of stream the sender intends to initiate) etc. to the socket. All these functions are documented in the wiki. 

3) [Section 5](http://httpwg.org/specs/rfc7540.html#rfc.section.5)

- This section deals with Streams and multiplexing them over the same TCP connection or socket. 

- This portion has been partially implemented. I tried making a non blocking version using copas for dispatching and creating a queue. It presently works with the `socket.select()` function from luasocket which it uses to wait on the socket to find out if it's ready to be read or written to. There are basic definition for send and receive functions. 

- There is a simple implementation of a priority queue. I set a priority flag and if it's set I send up that stream first. 

- Feature implementing the monitoring of stream states is also done which enables the module to be aware of the stream state and respond accordingly. 

4) [Section 6](http://httpwg.org/specs/rfc7540.html#rfc.section.6)

- This section deals with the different frame types and their definition. 

- This portion has been completely implemented. The `send_frame` function in the module supports all the 10 types of frames. 


*Section 7* deals with the error module for which I have added a basic module which can be found [here](https://github.com/whoami-nr/luasec/blob/dev/src/http2_error.lua).

*Section 8* deals with HTTP/2 connection management and deals just with specifying which frames have been used for what kind of requests and what to do when we receive a response. I used this section as a reference for implementing my HTTP/2 connection module. Other sections in the RFC are also just considerations and references for a good implementation.


One of the most important part of this was reading RFC's and learning to adhere to the specs. Also learnt a lot about debugging a network protocol implementation while getting to know the internals. More details about the implementation and references can be found in the [wiki](https://github.com/whoami-nr/luasec/wiki/Luasec-HTTP-2-Module). 

---

# Work to be done

- Remove the fake connection object from the HTTPS module which become pointless after the integration. 

- Make the connection module non blocking.  

- Try to merge the existing PR supporting the ALPN negotiation scheme. 


# Roadmap for the HTTP/2 implementation and future work

I have been implementing HTTP/2 based on the RFC going through it one by one. I took a lot of template code from the lua-http module which can be found [here](https://github.com/daurnimator/lua-http/)

[Section 7](http://httpwg.org/specs/rfc7540.html#rfc.section.7) deals with HTTP/2 error codes for which we have to implement a module specifying the same. Based on this module it also has to be linked with the existing implementation so that all the error's (essentially error messages) get redirected to it and we receive proper error messages for debugging effectively. 


[Section 9](http://httpwg.org/specs/rfc7540.html#rfc.section.9) deals with Additional HTTP/2 requirements like connection management, setting up and following a priority tree and connection reuse. 
