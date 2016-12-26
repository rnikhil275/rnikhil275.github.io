---
layout: post
title: A Secure Portknocking Implementation - Portsmith
comments: True
---

[Source](https://github.com/rnikhil275/Portsmith)

<a href="https://en.wikipedia.org/wiki/Port_knocking">Port Knocking</a> is a concept where the ports on a particular computer appear to be closed until a special packet/port knock sequence is established. It is a method of externally opening ports in a system by doing a sequence of connection attempts on a set of pre-specified closed ports. Once a correct sequence of connection attempts is made, the firewall rules are dynamically modified to allow the external system to connect to a specified port. This concept has been around for a long time and you can check out some implementations <a href = "http://www.portknocking.org/view/implementations">here.</a> 

# Why ?

I had a server on Digital Ocean(DO) which kept getting pwned and used for DDosing some poor soul. DO used to shut down networking for my node every four days or so. At least I think this was the case since I had some unauthenticated services running on it. I was using the server as a proxy with an open port on the server at all times. Maybe a botnet was spreading by scanning the network for vulnerable hosts and then exploiting them ? I am not sure. DO has to figure that out. 

Anyway, I decided to do something about it and when searching for a method to obscure networking services, I found PortKnocking.

The purpose of this was to prevent port scanners from scanning target systems for exploitable services. The ports appear closed unless the attacker sends the correct knock sequence/packet to the machine. Initially, it was supposed to be a series of connection attempts or knocks on a series of ports but this kind of mechanism was vulnerable to replay attacks. A person watching the network could easily figure out which ports are knocking before a connection is established. 

# Implementation

## Server side:

Instead of making the client ping a couple of ports, I decided to close all ports and log all connection attempts to these firewalled ports to /var/log/kern.log. I parse kern.log to to find my encrypted packet and authorize clients. 

First step would be creating keys for each client. I call this profiles. One user can have multiple laptops connecting to the same server.

	sudo python3 create-profile.py profilename portnumber

This creates a folder at '/etc/portsmith.d' and also a subfolder with the profile name. The subfolder contains two files. One is the encryption key which must be kept secret and other is the knockPort which the client has to knock.

The encryption key is a URL-safe base64-encoded 32-byte key. This must be kept secret. Anyone with this key will be able to create and read messages. This folder has to be transferred to the client computer securely using 'scp' or some other method.

After this, the server can start listening for knocks. 

## Client side:

The Knocker:
I use hping3 to craft TCP packets. The knock packet is encrypted using the key transferred from the server and then sent to the knockport. It gets logged into kern.log which is read by Portsmith. It is then decrypted and the required open is then opened for the sourceIP using a custom iptables command. 


# TODO

1) It currently uses <a href = "https://cryptography.io/en/latest/fernet/"> Fernet </a> Symmetric Encryption Library from the cryptography package. It's source and spec can be found [here](https://cryptography.io/en/latest/_modules/cryptography/fernet/) and [here](https://github.com/fernet/spec/blob/master/Spec.md) respectively. It uses:
*	AES in CBC mode with a 128 bit key for encryption; using PKCS7 for padding
* 	HMAC using SHA256 for authentication

This is a high level library. I would like to rewrite the cryptomethods using cryptographic.primitives instead. Maybe try out AES in CTR mode ? Either way, the crypto methods are going to be rewritten using low level (hazmat :P) functions. I think this would be good learning experience. 

2) Support for multiple profiles on the server. This is almost done.

3) Check and add user permissions when accessing directories, running system commands and changing iptables rules. 

4) Fork out the code which has to be run as root and separate it. This would increase security and take the project closer to be used in production.

5) Right now, it only opens ports. It should also close ports after a specified window if there is no successful connection. Also, handle a lot of exceptions and edge cases.

6) Make a daemon for running on the server. 

7) I had implemented a simple socks proxy. It performs the required knocks, makes sure the port gets opened before sending the application data to the particular server. Any application supporting socks proxy could technically use it but I couldn't get it to work properly. Work on the proxy.

7) REWRITE as a kernel module ???? 

