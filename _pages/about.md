---
layout: single
title: "About"
permalink: /about/
author_profile: true
---

Hey, I'm Ben, I'm a Senior Customer Support Engineer at Checkr. This site is where I document my homelab and, over time, the IT work I'm building around it.

## What's here

- **Posts** — build logs, troubleshooting notes, "how I set this up" writeups
- **Projects** — a more structured look at individual pieces of the lab
  (services, automation, infrastructure-as-code, etc.)

## The lab

A quick snapshot of what's currently running:

- **Hardware:**
  - HP MP9 G4 Mini-PC
    - CPU — Intel i5-8500T
    - GPU — Integrated
    - RAM — 8Gb
    - HD — 2x TEAMGROUP MP44L 1TB NVME
  - Lenovo Thinkpad
    - CPU — ?
    - GPU — Integrated
    - RAM — 8Gb
    - HD — Samsung 880 Pro SSD

- **Hypervisors:** 
  - Proxmox — The type 1 hypervisor everything is built on
  - Docker — More effecient containerization

- **Networking:** 
  - Peplink B-One Router — Fancy switches to come as the need presents itself

- **Core services:**
  - Pi-hole — Ad-blocking DNS server. Points to unbound
  - Unbound — Recursive DNS server
  - Tailscale — Secure remote access
  - Jellyfin — Media server
  - Home Assistant — Smart home automations
  - Paperless-NGX — Organized document storage
  - SMB share — Remote file storage

- **Monitoring:** 
  - Grafana + Prometheus — (Coming Soon)

- **Automation / IaC:** 
  - Docker Compose

## Why I'm building this

My homelab started with Pi-hole. I was curious how DNS actually worked under the hood. One ad-blocking DNS server turned into a deeper interest in networking and self-hosting. Since then it's grown into a study platform. As I've studied for certifications and picked up new skills, I've used the lab to actually build the things I'm learning about, rather than just reading theory. 

The goal now is to keep pushing it toward something genuinely resilient, available, and secure. I'm working to build the kind of environment that mirrors what's expected in a real IT/DevOps role. 

That mindset got tested directly when a config change to Unbound took down DNS for my whole network. I switched over to a backup resolver to restore connectivity, rolled back the change, and tested it properly before bringing my local DNS server back online. It was a small incident, but it's stuck with me as a reminder of why backups, staged rollouts, and testing changes before they hit prod are so important.

## Elsewhere

- [GitHub](https://github.com/sediment5193)
- [LinkedIn](https://linkedin.com/in/benjamin-nichols-11a809138)
