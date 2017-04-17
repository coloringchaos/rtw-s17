---
layout: post
title: "Digital Ocean"
date: 2017-4-17
categories:
description: 
permalink: /digitalocean/

---

A few of you have encountered this digital ocean error: 

![host key error](../img/host-key-error.png)


Here is the solution:

`ssh-keygen -R hostname`

Make sure you replace hostname with your droplet's IP address or domain name (whichever you use to ssh in).