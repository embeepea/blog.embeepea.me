---
layout: post
title:  "X11 Forwarding over SSH from a Headless VM"
date:   2014-01-21 15:13:58
categories: x11 ssh vm sysadm
---

I spent several hours yesterday and today trying to figure out how to
get X11 forwarding to work over an ssh connection from my MacBook to a
CentOS VM that I have running locally on the MacBook via VirtualBox.
I found lots of instructions on how to do this, on stackoverflow and
elsewhere, explaining that the sshd server (`/etc/ssh/sshd_config` on VM in
this case) must be configured with

    X11Forwarding yes

and the client (`/etc/ssh_config` on the MacBook in this case) must be configured with

    ForwardX11 yes

and that you should invoke ssh with the "-X", or maybe the "-Y"
option.  I did all these things, but it still wasn't working, until I
discovered the missing piece: *install xauth on the server*!!
