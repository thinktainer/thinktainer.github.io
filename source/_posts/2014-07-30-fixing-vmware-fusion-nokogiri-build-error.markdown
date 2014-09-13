---
layout: post
title: "Fixing vmware_fusion nokogiri build error"
date: 2014-07-30 19:24:27 +0100
comments: true
categories: [Vagrant, vmware, windows] 
---

When trying to install vagrant for the second time, I ran into an error about installing the nokogiri gem again. (This happened to me before, but I since forgot how I fixed it).
Thank god nowadays there is [stackoverflow](http://stackoverflow.com), so a solution to most problems is never far away.
This [post](http://stackoverflow.com/a/23635023/672760) describes that one only needs to export an environment variable when trying to install the [vagrant_fusion plugin](https://www.vagrantup.com/vmware).

		NOKOGIRI_USE_SYSTEM_LIBRARIES=1 vagrant plugin install vagrant-vmware-fusion

I hope I remember I have posted this the next time I upgrade my mac.

