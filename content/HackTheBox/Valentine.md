---
date: 2020-10-05
description: "This machine teaches us about the Heartbleed security bug (CVE-2014-0160) in OpenSSL cryptography library.It had a huge impact on the world"
featured_image: "/images/Valentine/cover.png"
tags: [
"Heartbleed",
"OpenSSL",
"Linux Machine"
]
title: "Valentine"
---


# Introduction
This is a Linux Machine that is medium-ish(ly) difficult.It was uploaded by *mrb3n*.

### Prerequisites
Some skills that would be handy in solving this box are 

* Directory Busting
* Enumeration
* Linux cmdline
* Base64 encoding and decoding
* Tmux

# Scanning
We start our scanning part with nmap

> nmap -A -p- -T4 -sC -Pn -oN nmap.txt -vv 10.10.10.79

{{< figure src="/images/Valentine/nmap.png" title="nmap results for the above command" >}}

Port 22,80,443 are only open.

Starting off with port 80, we find that there is only a pic on the homepage 

{{< figure src="/images/Valentine/omg.jpg" title="omg.jpg" >}}

Through reverse searching we find that this image is indeed an image used to spread awareness of Heartbleed bug.

*Whenever we find any webserver open, Directory Busting is always a nice idea.*

Using gobuster we find 4 pages that could be useful.

<!--Image of Gobuster-->

/dev contains files called notes.txt and hype.key

Opening notes.txt we find some text written

>To do:
>>1) Coffee.
>>2) Research.
>>3) Fix decoder/encoder before going live.
>>4) Make sure encoding/decoding is only done client-side.
>>5) Don't use the decoder/encoder until any of this is done.
>>6) Find a better way to take notes.



Opening hype.key we find 

![alt text](/images/Valentine/hype-key.png "title")

