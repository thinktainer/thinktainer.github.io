---
layout: post
title: "DNSMasq for using docker service containers in and out of docker"
date: 2017-02-11 18:56:11 +0000
comments: true
categories: docker dns
---

In order to be able to use services installed via [docker-compose](https://docs.docker.com/compose/) from both in-
and outside of the docker network I have started using [DNSMasq](https://wiki.archlinux.org/index.php/dnsmasq). This
lets me resolve hosts and services in the `.dev` domain from both inside and outside of the docker network. I have
created a [systemd](https://www.freedesktop.org/wiki/Software/systemd/) service definition, that starts DNSMasq after
the network comes up. It ties into [openresolv](https://roy.marples.name/projects/openresolv/index), so that the
configuration survives dynamic network changes (like vpn). here are the config files:

`/etc/systemd/system/dnsmasq-tld-dev.service`
{% gist 1797fad4e4de40e5087632eb3fcdcee7 dnsmasq-tld-dev.service %}

`/etc/resolvconf.conf`
{% gist 1797fad4e4de40e5087632eb3fcdcee7 resolvconf.conf %}

`/etc/dnsmasq-tld-dev.conf`
{% gist 1797fad4e4de40e5087632eb3fcdcee7 dnsmasq-tld-dev.conf %}
