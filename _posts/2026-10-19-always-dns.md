---
title: "It's literally always DNS"
date: 2026-10-19
categories: [Outage, Networking, Observability]
tags: [Proxmox, Pi-Hole, Unbound, Uptime Kuma]
excerpt: "A brown out took out one of my Proxmox nodes and my network with it"
---

## Overview

I got home one day and my internet didn't work. My devices connected, but I wasn't able to get online. First I pinged my defaut gateway. I got a reponse so everything was connected locally. Next I pinged 8.8.8.8 and got a reponse so I could tell my network was up and able to make outbound calls. Lastly I pinged google.com. I got an error, unknown host. This means that we're not getting a DNS resolution for the domain. Since I self-host my DNS server, I opened up Proxmox in a web browser. The page completely timed out.

I took a look at the Mini-PC that this node is running on. It was completely powered off. Powering it back on brought up the node and my DNS server which restored internet to my apartment.

This issue was probably caused by a momentary power outage as the entire Proxmox node was powered off, but none of my clocks reset. My other node, running on an old laptop, was still running just fine.

## Next Steps

Powering my server back on was a simple fix, but this incident made me realize I needed to setup a backup DNS server. This gives the network can failover to. I also decided to add uptime monitoring to notify me when something like this happens again. That way I can locate the issue more quickly in the future.

I realized running a server on an old laptop is an inexpensive way to incorporate what is in effect a UPS into my setup. It'd be better to run everything off of a UPS, but adding more hardware isn't in the budget for me right now.

So, working with what I have now, I cloned the container running my DNS server over to my laptop node. I gave it a DHCP reservation and setup all of my local networks to use that new server as a backup DNS resolver.

For observability, I got Uptime Kuma running. This tool is now constantly monitoring both of my DNS servers for outages. I'll be notified if anything goes offline. I've been wanting to get something like this set this up for a while now. This incident gave me a good excuse to sit down and do it. [More on how I got that going can be found here.](/homelab-site/projects/uptime-kuma/)
