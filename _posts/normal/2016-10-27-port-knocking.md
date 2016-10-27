// ---
// layout: post
// title: Port Knocking - Portsmith
// comments: True
// ---

// Recentrly I came across this concept of <a href="https://en.wikipedia.org/wiki/Port_knocking">Port Knocking</a>. It is a method of externally opening ports in a system by doing a sequence of connection attempts on a set of prespecified closed ports. Once a correct sequence of connection attempts is made, the firewall rules are dynamically modified to allow the external system to connect to a specified port. This concept has been around for a long time and you can check out some implementations <a href = "http://www.portknocking.org/view/implementations">here.</a>  

// The purpose of this was to prevent port scanners from scanning target systems for exploitable services. The ports appear closed unless the attacker sends the correct knock sequence to the machine.

// Initially, it was supposed to be a series of connection attempts    
// Security by Obscurity
// Defense strategy meant to be simple to implement and use with minimum interfernce from sysadmin
// Prevent simple malware spreading
// After going through a bunch of implementations, I decided on a few things:
// We can use cryptography to simplify the process of port knocking
// * The performace requirements of the daemon should be minimum and there should be a minimum overhead when it's running.
// * Everything should be encrypted and it should at least be safe from replay and man in middle attacks. Network sniffers shouldn't be able to figure out the sequence.
// * Cross Platform with less dependencies and easy to use

// # Why is this necessary ?
// Portsmith Features:

// Usability
// Portsmith-proxy