---
layout: post
title: Build a tracer with ptrace - microTracer
comments: True
ready: False
---

I wanted to get to know how strace works and to really understand it's working, I decided to write my own implementation which does what strace does although on a simple level. I won't be explaining about what strace does because there are like a million entries on the Internet which explain the same. I shall be explaining about my implementation. 

# Microstrace

understand how strace interjects itself between kernel and the userspace program
ptrace system call
	trace system call
	write and read from registers and memory
	manipulate signal delivery to the process
	used by gdb also
why strace 
	find which config file a program uses
	sandboxing - find which all shared libs a program uses
	common example for sniffing password: ssh process pass the incoming pass to the main sshd process

