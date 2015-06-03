---
layout: post
title: "Running Powershell from FAKE"
date: 2015-06-03 15:16:01 +0100
comments: true
categories: SDLC Powershell F# FAKE
---

At my job we had a requirement for connecting one of our systems to a new
sql backend. As there are several other existing solutions in our code base that
already do schema migrations and db initialization I didn't get to use
[dbup](http://dbup.github.io), which would normally be my go to solution for
doing this kind of job in .NET.

I'm strongly convinced that being able to run the same build on the build server
as on your local machine is a powerful thing, when it comes to troubleshooting
issues with it. So I generally abstain from putting any logic of *how* to build
on the build server, and rather let it figure out *when* to do it.
So I was stuck with powershell, and I wanted to make the existing scripts with
our build tool of choice: [FAKE](http://fsharp.github.io/FAKE/).

It turned out to be quite a task, as all the libraries in the
[System.Automation.Management](https://msdn.microsoft.com/en-us/library/System.Management.Automation(v=vs.85).aspx)
namespace looked frighteningly unfamiliar, and were obscured with the typical
windows security features that make it so hard to understand the usage patterns
for a Microsoft Windows Framework. I'm sure I'll have forgotten how to do this
tomorrow, and I don't want to go through the process of piecing it together
again, so I'm adding this post for future reference.

Here is the code:
{% gist f75e4492cc193b4bd9d8 %}

One thing that is noteworthy is that even though the framework threw an
exception after the script's execution (which performed it's task perfectly),
this didn't bubble up to FAKE, so it's not fit to stop the build in case of
failure, at this point.

One of the things that you need to address when executing powershell scripts in
a non-interactive host, is that the `Write-Host` cmdlets you may be using will
no longer work, as they're effectively trying to write to console. I was able to
resolve this by changing them to `Write-Output`, after which the exceptions
stopped for me.

That is all.

