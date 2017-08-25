---
layout: post
title: Cryptopals Challenges Set 1 Solutions
comments: True
ready: False
---

# Set 1 - Basics

These are my solution to the first set of 8 problems which are:

* Convert hex to base64
* Fixed XOR
* Single-byte XOR cipher
* Detect single-character XOR
* Implement repeating-key XOR
* Break repeating-key XOR
* AES in ECB mode
* Detect AES in ECB mode

I have linked the problem's actual page in the headings. Read them before going to the solution. I also suggest that you solve these on your own first before looking at solutions. 

1) [Convert hex to base64](https://cryptopals.com/sets/1/challenges/1)

What we are initially given is a hexadecimal representation of the binary data which can be converted to raw bytes using the binascii module's unhexlify function. Now that we have the raw bytes, we just need to use base64 encoding to represent them and then convert to string for comparing with the solution. 

[Solution Here](https://github.com/rnikhil275/crytopals-solutions/blob/master/set-1/1.py)

2) [Fixed XOR](https://cryptopals.com/sets/1/challenges/2)

Strings in Python 3 are unicode objects. Converting the strings to base 16 integers, XORing them, then converting back gives us the solution

[Solution Here](https://github.com/rnikhil275/crytopals-solutions/blob/master/set-1/2.py)

3) [Single-byte XOR cipher](https://cryptopals.com/sets/1/challenges/3)

We are again given an hex encoded string but this time, it has already been XOR'd against a single character. The Goal is to find the key and decrypt the message. 

The first challenges were mainly warm ups and this one actually involves breaking a (simple) cipher. We could brute force it but as suggested in the challenges page, let's use character frequency as a metric to break this. I found  a statistical distribution for English text [here](http://www.data-compression.com/english.html). Since this XOR operation modifies modifies each byte by the same amount, the frequency distribution won't change. 

I got some erros while decodes bytes objects into 'utf-8' but when I set it to ignore, I nevertheless got the answer. I should look into this again.

## move page from drafts

