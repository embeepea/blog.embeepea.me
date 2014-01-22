---
layout: post
title:  "X11 Forwarding over SSH from a Headless VM"
date:   2014-01-21 15:13:58
categories: x11 ssh vm sysadm
---

I spent several hours yesterday and today trying to figure out how to
get X11 forwarding to work over an ssh connection from my MacBook to a
CentOS VM that I have running locally on the MacBook via Vagrant and
VirtualBox.

I need this because I'm using python matplotlib to display plots, and
I want to do this on a VM rather than directly on my MacBook, since
I'm experimenting with installing various other python libraries, and
I don't want to run the risk of screwing up the python installation(s)
on my MacBook by installing experimental libraries.  I tried using
virtualenv on the MacBook but could not get it to work.


I've done X11 forwarding over ssh many times before, but for some reason
this time it wasn't working. I found lots of instructions on how to do it, on
stackoverflow and elsewhere, explaining that the sshd server config file
( `/etc/ssh/sshd_config` on the CentOS VM in this case) must contain

    X11Forwarding yes

and the ssh client config file ( `/etc/ssh_config` on the MacBook in this case)
must contain

    ForwardX11 yes

and that you should invoke ssh with the "-X", or maybe the "-Y"
option.  I did all these things, but it still wasn't working, until I
discovered the missing piece:

  **install xorg-x11-xauth on the server !**

Apparently xauth has to be present on the server (the VM in my case),
because when your local ssh client attempts to set up X11 forwarding, it
uses xauth to set up the authorization information used to connect to your
local X server.  So, in addition to the above ssh/ssd config changes,

    sudo yum install xorg-x11-xauth

on the VM did the trick.

The way that I discovered this was by stumbling on the `-vv` option for ssh,
which causes it to print out all sorts of wonderful debugging details, part
of which contained a message about xauth not being found.
