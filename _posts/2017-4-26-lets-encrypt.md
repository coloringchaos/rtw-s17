---
layout: post
title: "Let's Encrypt"
date: 2017-4-26
categories:
description: 
permalink: /lets-encrpyt/

---

Let's Encrypt can be used to easily obtain a free SSL certificate, which can be installed manually, regardless of your choice of web server software.

***You must own or control the registered domain name that you wish to use the certificate with.*** Follow these instructions for setting up a [custom domain with digital ocean](/rtw-s17/domains). 

After you do that, you can use [Let's Encrypt manually](https://certbot.eff.org/docs/using.html#manual). Certbot is software client used by Let’s Encrypt that simplifies the process of enabling encrypted https on web servers.

Here's a guide from [Digital Ocean on setting up Let's Encrypt](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-14-04)

Finally, you'll need to modify your server code to include a certificate and a key. 

More coming soon 🤙