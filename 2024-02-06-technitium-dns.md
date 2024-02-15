---
layout: post
title: Technitium DNS
date: 2024-02-06 15:27 -0500
categories: [Homelab]
---

# Technitium: DNS level adblocking! Private + Secure DNS!

I ended up ignoring my own to-do list and jumped to some new random project that caught my eye (this will likely be a common trend). While uBlock is great for blocking ads, I really like the idea of having a bit more control over DNS in my network. As such, setting up my own DNS server sesemed like a quick and fun project. 

## Ad Blocking
The first and most immediately noticable benefit is, of course, DNS level ad blocking. There are countless DNS blacklists available on the internet that can be used to accomplish this. For my purposes, I went with a list made by [Steven Black on GitHub](https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts). This file is a consolidation of several different host files which contain known adware and malware domains which are then blocked. uBlock Origin was good enough for accomplishing this in my browser, but this solution doesn't cover any DNS queries made by anything else on my desktop. This includes installed programs which try to send out telemetry or track my activities in some manner. For example, I've noticed that the Epic Games client reaches out to tracking.epicgames.com, this has since been blocked dozens of times. Besides my personal desktop, I can extend this adblocking capability to every other device in my environment without the need for some adblocking extension. 

## Privacy & Security
From a cybersecurity persepctive, the other main benefit comes in the form of enhanced privacy and security. DNS traditionally operates over UDP and is not encrypted. As such, anybody snooping on your network traffic can see all of your DNS queries, including your ISP! Some ISPs will violate your expectations of privacy and collect this data which they may then sell to third parties or use it for targeted advertising. The two most commonly used public DNS providers, Cloudflare and Google, are exceptions to this (Google gets a bad rep as far as privacy goes, but they have improved their privacy policy in recent years. Cloudflare is still my DNS of choice, however)
While most tech-savy users will opt to use 1.1.1.1, the DNS requests sent to Cloudflare still hop through your ISP, all in plaintext! The solution is to simply encrypt this DNS traffic. With Technitium, options are available to forward DNS queries out over TCP, TLS, HTTPS, and QUIC. The two most common options are to use either DNS-over-TLS (DoT) or DNS-over-HTTPS (DoH). Cloudflare explains it best [here](https://www.cloudflare.com/learning/dns/dns-over-tls/#:~:text=DNS%20over%20HTTPS%2C%20or%20DoH,forge%20or%20alter%20DNS%20traffic.), but I will try to sum it up. Both options will encrypt forwarded DNS queries. The main difference is that DoT is sent over port 853, whereas DoH is sent over 443. So with DoT, the people snooping on your traffic can't see the contents of your DNS queries, but they are still able to identify those packets as being DNS. DoH traffic on the other hand blends in with other HTTPS traffic. There are many arguments for DoH over DoT and vice versa, but for my purposes DoT is perfectly fine.


## Problems I Faced
Everything worked fine after changing the DNS of my desktop to my Technitium server. However, I hit a wall as soon as I tried changing the preferred DNS of the other devices on my network. Eventually I realized that I had forgotten to add firewall rules in OPNsense to allow other network segments to reach Technitium. SC of rule below****


https://www.cloudflare.com/learning/dns/dns-over-tls/