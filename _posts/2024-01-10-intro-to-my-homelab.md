---
layout: post
categories: [Homelab]
title: Intro to My Homelab
date: 2024-01-10 17:30 -0500
---

## Background

I've always wanted a dedicated computer to run servers for some of the games that I play. As I have learned more about cybersecurity over the last few years at Purdue, I found that that I also wanted a better VM hosting solution than the free options offered via AWS and Azure. As such, the idea of building a designated computer to do these things had always been in the back of my mind.

During fall of last year (2023), I came across a post on LinkedIn by [Michael Taggart](https://www.linkedin.com/in/mttaggart/) announcing his [Homelab Almanac](https://taggartinstitute.org/p/the-homelab-almanac) which walks through the process of getting started with personal virtualization. It sounded like it aligned perfectly with everything that I wanted to do with a Homelab, so I quickly bought a copy I was then determined to build a machine. I found some good Black Friday deals, bought the parts, and threw it all together.  

The parts list is available here:  
[https://pcpartpicker.com/user/cdup07/saved/thTK7P](https://pcpartpicker.com/user/cdup07/saved/thTK7P)

## Current State

I followed the steps described in the Homelab Almanac pretty closely, so I won't go into crazy detail describing my current virtualization/network architecture. At some point I will draw up a proper network diagram, but that sounds like a lot of work right now (I will do it eventually I swear). Through my Purdue courses I've developed a solid understanding of pfSense and VMware vSphere so the shift to OPNsense and Proxmox was manageable, though I found learning my way around Proxmox was still a bit challenging. To summarize what I currently have setup, the server is built on Proxmox which hosts a variety of VMs. Using OPNsense as a router/firewall solution, I then have a few different network zones setup including a DMZ and a couple extra private zones. Theres As far as the VMs themselves go I have a small Windows environment that includes 2 DCs and 2 standard Windows desktop machines, Kali, Metasploitable, a jumphost, and a game server. I also still use AWS for hosting my discord bots; I figured it's good to stay familiar with these cloud services. As per the Homelab Almanac I also have Packer, Terraform, and Ansible for configuring, deploying, and provisioning new VMs. Outside of use cases found in the book, I've only really used these DevOps tools for quickly spinning up new Windows VMs for hosting various game servers. I think it's important that I familiarize myself with these tools so that I have at least a bit of knowledge/understanding of DevOps processes. As such, as I continue to expand upon my lab these I will do my best to make use of these tools. 


One thing that I have had issues with is setting up a VPN through OPNsense. Even though I set up several VPNs in pfSense last semester for a lab, I have been unsuccessful doing it in my homelab. I've tried various client access solutions including OpenVPN, WireGuard, and I even tried IPsec (yuck!). I think there must be some issue with the network in my apartment complex, or maybe I'm just missing something obvious. This is something that I will revisit later. Currently, my solution for remotely accessing my network is a bit janky, but it works. On my jumpbox, I'm using a Cloudflare tunnel to serve RDP over my domain without opening up any ports. Then, I have protected access to this by using Cloudflare Zero Trust Access. Originally I had it set up so that users trying to access the RDP URL were hit with a Cloudflare portal which required that they enter an email address so that they could be sent an OTP. However, an email would only ever be sent if my own email address was entered. This made it so that I was the only user that could authenticate through this process (in theory). This quick and dirty solution was fine for the time, but I didn't really like it. In the last couple days I went and added Google as an identity provider so now users are required to sign in with Google. Naturally, only my Google account is allowed access. While I would still prefer a VPN, I'm content with this setup for now.


There are a couple extra things that I haven't mentioned but I think this mostly sums up everything that I currently have going on.


## Future Plans

- **SIEM / IDS**  
    This is the next major task I will take on. I imagine that I'll go with some combination of ELK stack and Security Onion, but I will do some more research before I decide. 
- **VPN** (hopefully)  
    Ideally, I will get Wireguard or OpenVPN working. 
- **Honeypot**  
    The scope of this will be decided later.

## Resources

[Connect to Remote Desktop through Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/use-cases/rdp/)

[Integrate Google authentication with Cloudflare Access](https://developers.cloudflare.com/cloudflare-one/identity/idp-integration/google/)