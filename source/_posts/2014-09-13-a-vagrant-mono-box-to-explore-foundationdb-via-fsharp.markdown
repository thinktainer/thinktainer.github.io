---
layout: post
title: "A Vagrant mono box to explore FSharp on linux"
date: 2014-09-13 16:15:41 +0100
comments: true
categories: vagrant fsharp mono foundationdb
---

Today I am sharing a ubuntu-64 trusty [Vagrant](http://www.vagrantup.com)
[box](https://vagrantcloud.com/thinktainer/boxes/trusty64-vmw-fsharp-foundationdb)
in order to make it easy to get started with mono development on linux.

**tl;dr** `vagrant init thinktainer/trusty64-vmw-fsharp-foundationdb`

## Why I think it matters

I have recently become a big fan of Microsofts ML language
[FSharp](http://www.fsharp.org). In my opinion one of the things that is holding
wider adoption of it back is the lack of a good cross platform story, like what
for instance [clojure](http://clojure.org) or [java](https://www.java.com/en/)
have to offer. *Minor niggle here, about the story for ruby and python on
Windows being far from great too.* One thing that has traditionally been painful
is the fact that the [mono](http://www.mono-project.com) runtime was a bit of a
pain to get recent packages for. Without @tpokorras efforts with his high
quality mono-opt packages for all major linux flavours it would have meant
a developer would have needed to compile the runtime environment herself, in
order to get a recent version on her machine (in a non-standard installation
folder).  Obviously with the advent of [Xamarin](http://xamarin.com)'s great
cross platform tools and those being based on the mono runtime things have
started looking up a lot since then.

## Microsoft wants us to use their languages and tools

I am convinced that the current trend for using linux as a .NET host is going to
gain even more momentum. Microsoft may be interested in such a development due
to the fact that Xamarin's cross platform story is a great way to channel people
down to the Azure platform as the cloud provider of the services for these
Xamarin apps. Another area where Microsoft is leaving the comfort zone of
Windows and actively seeking cross platform support by their framework is
[ASP.NET vNEXT](http://www.asp.net/vnext).  Again this may be useful for
Microsoft if they manage to get people onto their cloud platform by offering a
broader bandwith of choices, but in my opinion it is too early to tell if this is
in fact a vaible assumption. Regardless of what we are able to observe now, I
believe that Microsoft's cross platform efforts are going to be a great
investment in the long run, as it will make their services and potentially
devices more appealing to a wider audience of developers.

## FSharp is the ugly duckling at Microsoft

Microsoft has one of if not the best solution for developing object orientated
software on a vm, namely
[CSharp](http://en.wikipedia.org/wiki/C_Sharp_(programming_language)).  It has
been widely adopted by the public sector and the enterprise. For hobbyists and
'recreational programmers' not so much, at least that I'm aware of. A great
amount of resource at Microsoft is dedicated to the advancement, refinement and
marketing of this tooling, and rightly so.

The story for FSharp at Microsoft is different. One example where Microsoft was
marketing C# and to an extent C++ and javascript was the
[Build](http://www.buildwindows.com) conference in 2014. During the whole
conference there was not one single talk about F#. The tooling and integration
support for using F# leaves a developer wanting in a number of places. The most
obvious one in my opinion is that one has to ship the latest FSharp.Core
libraries as a compiled artifact when trying to host applications using them in
azure. These are windows boxes maintained by Microsoft, so it strikes me as odd
that the F# framework on these boxes is not the latest version available to
developers. There are more areas like sponsoring where Microsoft is hardly
visible when it comes to promoting F#. I find it hard to find reasons for why F#
is being left in the background to this extent. I am entertaining two possible
explanations, which are both purely speculative and quite absurd. 

## This is great for the open source movement

Due to the relative apathy of Microsoft in regards to engaging with the
community around FSharp, the community prospers. There is a number of problems
that have been solved by the FSharp open source community in a manner of great
technical excellence. Examples of this would be the number of highly useful type
providers, data analysis solutions and parsers.

The core language is open source, and accepting contributions from the
community. This is big news for a language that was developed under the umbrella
of a big corporate like Microsoft. Under the vision of Don Syme, who is not only
the father of F#, but also responsible for some of the key features that make C#
so attractive I believe there is a bright future for the language ahead.

The lack of percievable involvement from Microsoft in the current affairs of the
language makes it more palatable to people who feel an ingrained antipathy for
whatever reasons for the company. There is a great number of fantastic
programmers in this group, and it is a highly desirable to get some of them 'on
board', in order to start spreading the word.

A key ingredient to further the spread of F# in these communities is being able
to use it without having to use the Windows operating system.


I am hoping that my contribution will help this process along.

