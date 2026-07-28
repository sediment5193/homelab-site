---
title: "Localized DNS Petcam"
date: 2026-07-27
categories: [networking, physical-security]
tags: [docker, home-assistant, frigate]
excerpt: "My plan to build a pet cam service without breaking the bank."
---

## Overview

My wife and I recently got a cat named Ollie. He's super adorable. When we're away though, we want to keep an eye on him. So, my next project will be to build a pet cam.

## Requirements

My main concerns with this are: security, reliability, and pricing.

### Security

My first goal is to make sure our camera doesn't wind up on one of those hacked feed websites where you can just freely watch random misconfigured security cameras / pet cameras. For now, I'll just keep everything safely in my LAN and phone home with a VPN. Maybe this will change down the road.

### Reliability

I also want to make sure that this thing actually works even when we're across the country. Using a trusted camera and NVR combo is a good start. I'd also like to implement uptime monitoring and ensure everything is tested out before we spend time away to identify gaps.

### Pricing

To keep things practical, I also want to make sure I'm not having to buy endless TBs of storage to hold all the footage. I just don't have the money.

### Other

Lastly, some sort of Home Assistant integration would be great. I've seen some pretty cool setups with automations that involve cameras. I'd like to play with this down the road as well.

## The Plan

### Frigate NVR

Very quickly in my search I came across [Frigate NVR](https://frigate.video/), an open-source network video recorder that ticks all the boxes and more. Everything runs completely locally, it's a popular project relied on by many to record video, and it's got a lot of clever ways built in to only store the footage that matters. Last, but not least, it's got a very strong Home Assistant integration.

### The Camera

To keep things simple, I'll start with a single camera recommended to me by a friend, the [Reolink E1 Pro](https://www.amazon.com/dp/B08RS4C67L). This supports RTSP which allows it to connect to Frigate and outputs dual video streams. One stream is the original quality, full-res video, the other is a low-res copy.

### Why This Should Work

The low-res copy stream, with frame rates as low as 5 FPS, can be routed to Frigate's AI object detection engine. This will hopefully give my humble HP MP9 G4 breathing room to run image recognition continuously. 

The object detection engine can be configured to trigger a video recording when a target from a preset list is seen. Once the target is seen, the full-res video stream is recorded. The NVR also has retention settings to prevent this bank of recordings from growing too large and filling up hard-drives.

Here's a visual breakdown of this process:

<div class="mermaid">
flowchart TD
    A[Reolink E1 Pro]
    A -->|Low Res Stream| B(Object Detection)
    A -->|Full Res Stream| C(Video Recording)

    subgraph Frigate
        direction LR
        B --> C
    end

    B --> D(Home Assistant)

    C --> E(Docker)
    D <--> E
</div>

With this setup, I can minimize storage space while keeping up with what our cat has been doing while we're away. Using this [calculator found in Frigate's documentation](https://calculator.ipconfigure.com/), I feel confident in setting aside just 100Gb for footage storage.

From there, Home Assistant has the ability to display live feeds, recorded videos, and use detection data to trigger automations. That's the plan for now. I'll keep things updated with progress.

## Next Steps

The obvious next step from here will be to buy a camera. Once I've done that and configured everything I'll have a better idea of how well my current hardware can run the object detection AI. My hope is that things will run smoothly with just 1 camera. If things get sluggish or I wind up adding more cameras, I can add an external TPU to offload most of the heavy lifting from the AI processing.

Also, I need to do more research into uptime monitoring. This may be something Home Assistant can do for me, or I may need to setup something like Grafana or Uptime Kuma.
