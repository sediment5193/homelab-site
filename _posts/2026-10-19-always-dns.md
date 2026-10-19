---
title: "It's literally always DNS"
date: 2026-10-19
categories: [Outage, Networking, Observability]
tags: [Proxmox, Pi-Hole, Unbound, Uptime Kuma]
excerpt: "A brown out took out one of my Proxmox nodes and with it my DNS server"
---

## Overview

When I got home one day, my internet had inexplicably stopped working. First I pinged my defaut gateway. I got a reponse so everything was connected locally. Next I pinged 8.8.8.8. I got a reponse so I could tell my network was up and able to make outbound calls. Lastly I pinged google.com. I got an error, unknown host. This means that we're not getting a DNS resolution for the domain. Since I self-host my DNS server, I opened up Proxmox in a web browser. The page completely timed out.

I took a look at the Mini-PC that this node is running on. It was completely powered off. Powering it back on brought up the node and my DNS server which restored internet to my apartment.

This issue was probably caused by a brown out as the entire Proxmox node was powered off, but none of my clocks reset. My other node, running on an old laptop, was still running just fine.

## Next Steps

Powering my server back on was a simple fix, but this incident made me realize I needed to setup a backup DNS server the network can failover to and uptime monitoring to notify me when something like this happens so that I can locate the issue more quickly.

Additionally, I realized running a server on an old laptop is an inexpensive way to incorpoate a UPS into my setup. So, I cloned the container running my DNS server over to my laptop Node, gave it a DHCP reservation, and setup all of my local networks to use that new server as a backup DNS resolver.

For observability, I got Uptime Kuma running. I've been wanting to set this tool up for a while. This gave me a good excuse to do it. [More on how I got that going can be found here.](/homelab-site/projects/uptime-kuma/)
