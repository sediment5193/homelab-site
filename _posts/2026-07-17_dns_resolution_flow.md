---
title: "How My Local DNS Resolution Works"
date: 2026-07-17
categories: [networking]
tags: [dns, unbound, pi-hole]
excerpt: "A look at how DNS requests flow through Pi-hole and Unbound in the lab."
---

## Overview

The first things I set up in the lab was Pi-hole. Eventually, I added Unbound behind it
as a recursive resolver to give me more control over cached requests and prevent 3rd parties from having access to DNS requests originating from my network. 

## What it Does

Pi-hole handles ad/tracker blocklisting before anything reaches Unbound. Unbound does full recursive resolution instead of forwarding to a third-party resolver, so no upstream DNS provider sees my lookups.

Here's the path a DNS query actually takes:

<div class="mermaid">
sequenceDiagram
    participant Client
    participant Pi-hole
    participant Unbound
    participant Root/TLD/Auth

    Client->>Pi-hole: DNS query (e.g. example.com)
    alt Domain is blocklisted
        Pi-hole-->>Client: Blocked (0.0.0.0 / NXDOMAIN)
    else Domain is allowed
        Pi-hole->>Unbound: Forward query
        Unbound->>Root/TLD/Auth: Recursive resolution
        Root/TLD/Auth-->>Unbound: Authoritative answer
        Unbound-->>Pi-hole: Resolved IP
        Pi-hole-->>Client: Resolved IP
    end
</div>

## Challenges

This is also the setup that bit me once — see the [about page](/homelab-site/about/about/)
  for the outage story
