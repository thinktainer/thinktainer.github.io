---
layout: post
title: "'Bridged network for qemu in arch using systemd'"
date: 2016-03-17 14:55:40 +0000
comments: true
categories:
  - networking
  - virtualization
  - kvm
  - hvm
  - arch
  - linux
---

## The problem

I want to run a windows virtual machine inside my arch linux development
laptop, in order to be able to run Microsoft's [Visual
Studio](https://visualstudio.com), which I used to enjoy using massively in the
past, particularly in combination with Jetbrains' Resharper. Recently I have
gravitated more towards a lighter stack, preferring [F#](http://fsharp.org) over
C#. The tooling required to do the same kind of work in F# is much less than in
C# (depending on use case of course), and I have always favoured an approach
that lets me code using a text editor.

Enough rambling, so for this particular scenario I need VisualStudio, and by
extension Windows. I am going to go with Windows Server 2012 r2, as i find the
server Windows versions to be generally more amenable to running in a
virtualised environment. I also don't care much for news or music in a vm, so
running Windows Server doesn't limit me in any way.

## The options for virtualization

Considering the options, it basically boils down to Hardware-assisted
virtualization versus Full virtualization (Vmware, Parallels, etc). I have long
been on a mac as my main box, and have often felt the slowdown incurred by Full
virtualization solutions, so being on a shiny Arch Linux box, it was an easy
decision to go with Hardware-assisted (hvm).

The choice then is between xen and kvm, but I think the preparation for the xen
environment is a lot more intrusive than the kvm option. I also found a [recent
performance
comparison](https://major.io/2014/06/22/performance-benchmarks-kvm-vs-xen/) that
essentially showed no major performance difference between the technologies.
_Bring on the flame wars in the comments ;-)_

So kvm it is going to be for me.

## Installation

Installing the required couldn't be easier given arch's excellent package
manager. `sudo pacman -S qemu` brings all the required bits onto my machine,
and looking through the installation instructions in the arch wiki shows that I
already have all the required kernel modules on my machine. So far (I'm 2 months
in) the experience on arch with regards to package quality and packages being up
to date has been absolutely brilliant, so I'm still in my honey moon with this
distro.

## Networking

Here be dragons :-)

Considering the options for the network it becomes evident that there two ways
about it:

- The fully integrated solution, that makes it really easy on the user, by
  effectively creating vlans and bridges in the kvm / qemu stack, and hiding
  that completely from the user. This solution doesn't allow to have an
  optimised interface in the guest, and as a result of that is slower than the
  other solutions.
- The bridged networking version, which pushes most of the management complexity
  onto the user, but provides the benefit of a virtio interface, that maps the
  io directly to the host, and is much faster as a result.

Thinking _how hard can it be_ I decide to take a stab at the bridged networking
option. Now this is a bit of a pain on a laptop, if you want the guest os
(windows in my case) to be able to call out to the network. This is a solid
requirement in my case, for being able to get windows updates, etc.

The reason it is difficult is that wireless interfaces in linux don't allow
bridging on the kernel network stack (promiscuous mode), which means that they
can't be slaved to the bridge.

The alternative is to create a bridge device and forward the guest's traffic via
network address translation (NAT), to the wifi interface, using the linux
firewall (iptables).

Having NetworkManager as my main mechanism for connecting to wifi (I find the
gui way a bit easier than arch's preferred netctl), I need to come up with a way
of making the bridge start on system startup, and when I start the vm for the
interface to attach to the bridge. The attaching bit is pretty much handled for
me in recent versions of kvm, where I can give it a flag with a bridge
interface, and it will handle the rest.

So I decide to buy into the brave new world that is systemd, and think I'll be
able to use this knowledge in the future, as that behemoth seems to not stop for
anything, and given the engineering power behind it, I would be surprised to see
it go away any time soon.

### Create the bridge, and start on system startup

To create the bridge device I add the file

`/etc/systemd/network/qbr0.netdev`:
{% gist 45a09a798026a7b6a147 etc-systemd-network-qbr0.netdev %}

For it's network configuration I add:

`/etc/systemd/network/qbr0.network`
{% gist 45a09a798026a7b6a147 etc-systemd-network-qbr0.network %}

In order to enable the network part of systemd I need to enable the
systemd-networkd daemon. `systemctl enable systemd-networkd`. Simple enough.

### Allow qemu to connect to the bridge

In order for qemu to be allowed to use the bridge I add the file
`/etc/qemu/bridge.conf`, containing only the line: `allow qbr0`.

### Add dnsmasq to provide ip's to guests

In order for the guests to be able to receive ip's I add dnsmasq to serve ips on
the bridge device.

I add the configuration file: `/etc/dnsmasq-qbr0.conf` (apologies for top level
folder, this is all still a bit rough around the edges).

{% gist 45a09a798026a7b6a147 etc-dnsmasq-qbr0.conf %}

Note the assignment of the fixed ip, this is so that I can reliably rdp to the
same instance without having to go down the dns route.

I also add a service to systemd, to make this start up on system start (adding
dependencies to the network service to have started before):
`/etc/systemd/system/dnsmasq-qbr0.service`

{% gist 45a09a798026a7b6a147 etc-systemd-dnsmasq-qbr0.service %}

### Adding a start script for the kvm instance

Finally I modify [a
script](http://ubuntuforums.org/showthread.php?t=2289210&p=13334367#post13334367)
from the ubuntu forums by the awesome okky-htf, shout out, couldn't have done it
without you!

This is what I run to start the vm:

{% gist 45a09a798026a7b6a147 start.sh %}

I have set up rdp in the guest, hence the graphics are disabled at the bottom.
For the initial installation plus boot the start up order needs to be changed,
and the drives for the cd iso's need to be enabled.

I use this script to rdp into the instance, and am very happy with the
performance and stability of the set up.

{% gist 45a09a798026a7b6a147 connect.sh %}


This is all, I hope someone finds this useful, I'm mainly leaving it here for
myself, to be able to repeat this in the future. Comments (particularly
alternatives for the networknig) are very welcome.



