---
layout: post
title: "How to lead a computer astray on the world wide web"
date: 2013-10-03 13:15
comments: true 
categories: 
- Tutorial
- Host file
---

##In other words: How to troll your friends' computers

This is a guide on how you edit the host-file in order to map a domain to an ip-address. 

When you type in a domain name, such as [www.timflach.com](http://www.timflach.com/), you are actually being redirected to an IP-address. Around the world there are important computers that know what IP-address is mapped to what domain name. These are called DNS servers. They are updated when a domain name gets moved and real autorities on where web sites are located.

But, before your browser travels out into the world to collect information from the DNS servers it actually asks someone else. Or rather, it looks in your host file.

The host file is located in c:\windows\system32\drivers\etc\hosts (on Unix, /etc/hosts) and can contain information on where domain names lead. In it you can add pairs of domain names <-> IP-adresses to say that if a user types a specific domain name, the browser should be redirected to a particular IP. For example you can add the IP 
50.112.105.59 to the domain minecraft.net.

###Here is how you edit the host file!
Open the file. You might need to do so as an administrator. I found that the easiest way was to type

``` 
notepad c:\windows\system32\drivers\etc\hosts
```

after you've opened Windows' metro mode and then right click --> open as administrator.

You will now see a file that looks somewhat like this: 

```ps1
# Copyright (c) 1993-2009 Microsoft Corp.
#
# This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
#
# This file contains the mappings of IP addresses to host names. Each
# entry should be kept on an individual line. The IP address should
# be placed in the first column followed by the corresponding host name.
# The IP address and the host name should be separated by at least one
# space.
#
# Additionally, comments (such as these) may be inserted on individual
# lines or following the machine name denoted by a '#' symbol.
#
# For example:
#
#      102.54.94.97     rhino.acme.com          # source server
#       38.25.63.10     x.acme.com              # x client host

# localhost name resolution is handled within DNS itself.
#    127.0.0.1       localhost
#	::1             localhost

```

And here is where you can add your own customization. Write the line
```powershell
178.79.143.210		minecraft.net
```
save, visit the domain namne in your browser and see what happens.

When is this useful, except for trolling your little brother? Say, for example that you are located on a network where you can't access certain domain names without typing the IP-address. Now you can create your own way around that!


