---
layout: post
title: Securing NGINX with LetsEncrypt on Debian based Linux
categories: [Linux, Debian, NGINX, LetsEncrypt]
tags: [SSL, LetsEncrypt, NGINX, Debain, Linux, HowTo, Guide]
#published: false
---

## Installing Certbot

The first command to run to obtain an SSL certifcate is to install Certbot
```bash
sudo apt install certbot python3-certbot-nginx
```

## Obtaining an SSL Certificate

Certbot has many ways of obtaining an SSL Certificate and you should take some time with Certbot to find out what else it has to offer after this guide.

When your ready type the following:
```bash
sudo certbot --nginx -d example.com -d www.example.com
```

Thats it, your all done. Rember to renew your certificates before the 90 days are up or stay tuned for a guide on how to automate that here.