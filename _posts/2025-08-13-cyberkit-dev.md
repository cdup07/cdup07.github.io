---
layout: post
title: CyberKit.dev
date: 2025-08-13
---

### The Problem

After starting as a security analyst, I consistently found myself manually typing out OSINT details for the IPs, hashes, and domains I came across in alerts. Usually something along the lines of this:

```
OSINT on [1.1.1.1]:  
VirusTotal: 0/94 | CLOUDFLARENET  
AbuseIPDB: 0% Confidence | cloudflare.com | Content Delivery Network  
AlienVault: 0 Pulses | Known False Positive  
Location: Australia  
```

This was a tedious time sink - so Luke Albertson & I automated it.

### The Solution

Having already setup my domain and website through Cloudflare pages I explored what other services and capabilities were available. This lead to my discovery of Cloudflare 'Workers'. As per the AI summary I just got from Google, "Cloudflare Workers are a serverless execution environment that allows developers to deploy code globally at the edge of Cloudflare's network". Building my tool with Workers allowed me to make it globally accessible while also providing me with some cloud experience.  

**How it works:**

1. **User Input (Frontend)** – A React app on Cloudflare Pages lets users enter an IP, domain, or hash.

2. **Classification** – The frontend detects the artifact type (IP, domain/URL, or hash) using regex.

3. **Processing (Backend)** – Based on the artifact type, the request is routed to one of three dedicated Cloudflare Worker APIs (IP, domain/URL, or hash), each of which queries multiple OSINT sources (e.g., VirusTotal, AbuseIPDB, AlienVault, URLScan) and returns aggregated results.

![cyberkit.dev frontend](assets/img/posts/2025-08-13-cyberkit-dev/image-1.png)

Seconds later, the results are returned and are report-ready. 

![cyberkit.dev search results](assets/img/posts/2025-08-13-cyberkit-dev/image-2.png)

Check it out here:
[CyberKit.dev](https://cyberkit.dev/)
or here:
[osint.lukealbertson.com](https://osint.lukealbertson.com/)
or even here:
[osint.carsonww.com](https://osint.carsonww.com/)