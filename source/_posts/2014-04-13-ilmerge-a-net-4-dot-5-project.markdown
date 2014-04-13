---
layout: post
title: "ilmerge a .NET 4.5 project"
date: 2014-04-13 13:15:17 +0100
comments: true
categories: 
- .NET
- build
---
When packaging a console exe for easy deployment on a client machine I wanted to run ilmerge.exe recently. I have found that all it takes is to reference the right framework assemblies in the /targetplatform command line switch.
The full command on my machine then looks like:
``` ps1 ilmerge for .NET 4.5
ilmerge /t:exe /targetplatform:"v4,$env:windir\Microsoft.NET\Framework64\v4.0.30319" /lib:. /out:outname.exe .\inname.exe [additional-library.dll, ...]
```
