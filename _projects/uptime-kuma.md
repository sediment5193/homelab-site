---
title: "Uptime Monitoring and Observability"
date: 2026-10-19
category: "Network/Infra"
excerpt: "Now you're always on-call"
header:
  teaser: /assets/images/tmp-teaser.png
tech_stack: [Proxmox, Ubuntu Server, Docker, Uptime Kuma]
---

## Overview

A brown out at my apartment took down the Proxmox Node hosting my DNS Server. When I got home, there were no other signs of the power loss, but my internet stopped working. This is when I realized that in addition to a backup DNS server, I need uptime monitoring to notify me when something like this happens so that I can mitigate it more quickly. This is where Uptime Kuma comes into play.

Uptime Kuma is a self hosted observability tool you can use to keep tabs on the status of all your self-hosted services. Notifications can be setup to fire when a monitor tiggers so that when something needs attention, you knwo right away.

## Stack

This was deployed using Docker and Docker Compose in Ubuntu running on my Proxmox cluster.

## Installation

The install on this is extremely simple using Uptime Kuma's pre-made .yml file. I created a new directory for the service, then I pulled the .yml directly from the Uptime Kuma website. Lastly built it using Docker Compose.

<pre>
    <code>
        mkdir UptimeKuma && cd UptimeKuma/
        wget -O docker-compose.yml https://uptimekuma.co/wp-content/uploads/2026/04/Docker-Compose.yml
        docker compose up -d
    </code>
</pre>

## Setup

Get started with the setup process by pointing a web browser to the URL of your new service. It will start by asking you to setup a username and password. From there, the majority of the details needed autofill based on data from your host. The only selection I had to make was how I wanted the database to built. I went for the SQLite option since it's the default and simplest choice. The service literally builds the DB for you.

Once that's done, you're ready to setup monitors and notifications. I'm still in the process of setting up all of my monitors, but there are several monitor types to choose from.

1. HTTP/HTTPS
2. Ping Monitoring
3. Steam Game Servers
4. DNS Records
5. HTTPS Keyword Monitoring
6. Push Monitor
7. Port Monitoring

For more details on what each of these monitor types do, check out Uptime Kuma's documentation: https://uptimekuma.co/wiki/add-a-monitor-in-uptime-kuma/

Since this project was inspired by a brown out that took down my Proxmox Node that's hosting my DNS server, I started by adding two DNS record monitors pointing towards each of my DNS servers. 

- I set the monitor type to DNS
- Added a friendly name
- Set my hostname based on the actual host this is pointing towards
- Set the resolver server(s) to the IP of my DNS server
- Left the rest at default values

Thi resulted in 2 monitors showing the health of both of my DNS servers. I didn't want to be left constantly checking a status page though, so I setup notifications as well. There are even more notification delivery methods than there are monitor types and I am also not aware of any documentation that lists them, but I went with the Home Assistant delivery method.

First I went to User > Settings and clicked "Setup Notification". Then I entered my Home Assistant server's IP. Then I had to setup a long lived access token within Home Assistant. 

I opened home assistant and navigated to User > Security and created a new token. Be sure to copy this and don't lose it. Home Assistant only lets you copy this once.

From there, I went back to Uptime Kuma and pasted in my access token. Lastly, I setup the action that should happen in Home Assistant when a monitor fires. I went back to home assistant and used Settings > Developer Tools > Actions to find the right entity. I ended up using following format to ensure the notification only goes to one specific device.

<pre>
    <code>
        notify.mobile_[YOUR_DEVICE_NAME_HERE]
    </code>
</pre>

Finishing this off I clicked test and confirmed the notification fired on my mobile device Lastly, I checked "Default Enabled" and "Apply on all existing monitors" and saved.

## Challenges

The documentation for Uptime Kuma is pretty sparse, so I mostly had to rely on my own knowledge of how these types of tools work to get this going. Fortunately the setup was easy and the UI had helpful tool-tips.

The only real issue I ran into during the process was when setting up a monitor notification. Initially, I was getting a 400 error when clicking the "Test" button. This was caused by an invalid entity name in the "Notification Action" field I pre-pended my entity name with the action type from Home Assistant: "notify.". 

When I read the lengthy tool-tip a little more closely I noticed they specifically say to exclude this. Reading and following directions definitely helps. Once this was resolved, the notificatio fired normally. I tested the system by shutting down my backup DNS server entirely. Uptime Kuma caught it and fired the notification within the minute and pushed a notification to my phone right away.

Once I booted the server back up, the monitor went back to normal and I got another push notification advising me of such.

## Next Steps

The next step from here will be to setup additional monitors for each of my services I'm running. I plan on starting with all the Docker Containers first as a lot of these services are critical tools used on a daily basis. When ost of these drop offline, it's difficult to tell whether it's a problem with the client, network, or server. Additionally, with backup services like Immich which fails silently, you could go a long time without even realizing it isn't doing it's job.

From there I'll expand to less critical services that don't need as close of an eye on them. I'm excited for the additional observability and reliability this new service brings.

## Resources

- https://uptimekuma.co/install-uptime-kuma-docker/
- https://uptimekuma.co/wiki/add-a-monitor-in-uptime-kuma/
