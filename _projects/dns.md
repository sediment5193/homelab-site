---
title: "DNS Filtering and Recursive Resolution"
date: 2026-07-15
category: "Network"
excerpt: "Privately and securely complete the requests that are needed"
header:
  teaser: /assets/images/pihole.png
tech_stack: [Proxmox, Ubuntu Server, Pi-Hole, Unbound]
---

## Overview

The DNS on my network is self-hosted. There are two identical servers, one primary and one backup, each running on seperate nodes. DNS requests are firt handled by Pi-Hole which filters any requests to serve ads or tracking elements specified in my blocklists. Anything that makes it through is fed into Unbound, which completes the request directly via an authoritative DNS server upstream.

## Stack

- Proxmox
- Pi-Hole
- Unbound


## Pi-Hole

### Installation

To house everything I spun up a new CT running Ubuntu. You'll need to setup a static IP or DHCP reservation so you can point DNS traffic to a consistent IP. Now's a good time top handle that.

Then comes the standard update, upgrade, and reboot. Pi-hole's documentation provides a one-liner install script.

<pre>
  <code>
    curl -sSL https://install.pi-hole.net | bash
  </code>
</pre>

The script is extremely easy to use and walks you through installation. It will ask which upstream DNS provider you'd like to use. I selected Cloudflare just to get started. This is just to get you up and running and will change later.

From there it will ask if you want to use the sugested blocklist. Pi-hole only blocks requests that are on a blocklist so to get filtering right away I selected "Yes".

Then it asks if you'd like to enable query logging. I find this helpful for troubleshooting when Pi-Hole's filtering breaks sites so I opted into this as well. It will also ask which log level you'd like to use. I don't mind logging my network's traffic locally so I went with level 0 which shows everything.

After that the script takes care of the rest. If you're running a second DNS server as a backup.

### Setup

To actually make us of your shiny new DNS filter, you'll need to point your network towards it. Before doing that though, I visited https://canyoublockit.com/ (with my ad-blocker disabled) to get a baseline. Even the most basic test was riddled with Ads.

Now to get things running, you'll go into router's settings and set it to use your new server's IP for all DNS requests. Be sure to remove any backup DNS servers as this will result in filtered traffic still coming through.

Now toggle your device's network card off and back on again so the settings apply. Now you're ready to revist https://canyoublockit.com/. With the Pi-hole enabled, it already crushes the simple and extreme test. 

You can tune things up even further by adding more lists. you can do this directly from Pi-hole's UI. Point a web browser to the IP of your server and add "/admin".

When you start adding more lists, you're inhereintly more likely to block a critical component of a site you'd actually like to use. Fortunately you're able to whitelist domains within the Pi-Hole UI. Definitely do your own research to find a balance of blocking things you don't want without having to spend all your free time whitelisiting.

That being said though I added the following lists to the default suggested during setup:

<pre>
  <code>
      https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/pro.txt
      https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/fake.txt
  </code>
</pre>

With these lists, I'm able to block most ads before they're even requested, which seems to have sped up my network, while only having a few domains I've had to whitelist so far. Now that we know our DNS filter is blocking things we don't want to see and resolving things we do, we can take things a step further with Unbound.

## Unbound

### Installation

Unbound is also very easy to install. On the same CT we can install it via apt.

<pre>
  <code>
      sudo apt install unbound
  </code>
</pre>

Unbound requires a root hints file to operate. Installing directly from a package manager means this file is automatically managed and maintained.

### Setup

TODO

### Final Steps

Last, but not least, you'll want to clone this CT, ideally to another node. From there, you can setup another static IP or DHCP reservation and add the new IP address as your backup DNS resolver in your router's settings. That way, if your primary node goes down, you'll still be able to resolve domains to IPs.

## Challenges

Running DNS locally does mean that any issues with your DNS servers will, for all intents and purposes, take down your entire connection to the internet. This has happened a few times and I've written about it in more detail on the [about page](/homelab-site/about/about/) and in this [blog post](/homelab-site/posts/2026-10-19-always-dns).

I was especially susceptable when playing around with Unbound configs and blocklists before I had a second DNS server running. I'd definitely reccomend having a backup DNS server your clients can fail over to. Even if it's all running on the same hardware, it will allow you to make mistakes without taking out your ability to easily connect to the internet.

Additionally, I've found uptime monitoring to be extremely helpful. Before setting this up, I would find out something had failed by coming home from work and having no internet. Then systematically running ping commands to find out my DNS server was out. Uptime monitoring will give you a notification of an outage before you've even noticed. More details on how I have this setup can be found on [my Uptime Kuma project page](/homelab-site/projects/uptime-kuma).

## Next Steps

The setup I've described here does everything I'd like it to do and has been running smoothly for quite some time. I don't think I've ever said this about a homelab project to date, but I don't think there are any improvement to be made at this point. The next steps here are to forget about it until something else breaks.

## Resources

- https://docs.pi-hole.net/main/basic-install/
- https://docs.pi-hole.net/guides/dns/unbound/
