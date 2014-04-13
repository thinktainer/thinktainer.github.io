---
layout: post
status: publish
published: true
title: Fixing Xen Daemon on a Laptop with Nvidia Optimus graphics
author: mschinz
author_login: mschinz
author_email: m.schinz@gmail.com
date: '2012-04-21 17:12:12 +0100'
date_gmt: '2012-04-21 16:12:12 +0100'
categories:
- Hardware
- Linux
tags: []
---
When I tried to install xen on my new Inspiron laptop (15R N5110) - I ran into a bit of trouble with the pci probing of the xend daemon. It has something to do with the capabilities of the pci devices going into a loop or not being able to handle 2 graphics adapters. I found a similar problem [here](https://bugzilla.redhat.com/show_bug.cgi?format=multiple&amp;id=767742) and the same patch worked for me. I am posting this here again, so I don't forget and other people can find it.
The fix:
{% gist 2266335 %}
The error message:
{% gist 2266378%}
Hope this helps.

