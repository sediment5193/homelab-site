---
title: "Localized DNS Petcam"
date: 2026-07-27
categories: [networking, physical-security]
tags: [docker, home-assistant, frigate]
excerpt: "My plan to build a pet cam service without breaking the bank."
---

## Overview

My wife and I recently got a cat named Ollie. He's super adorable. When we're away, we want to keep an eye on him. So, my next project will be to build a pet cam.

## Requirements

My main concerns with this are: security, reliability, and pricing.

My first goal is to make sure our camera doesn't wind up on one of those hacked feed websites where you can just freely watch random misconfigured security cameras / pet cameras. For now, I'll just keep everything internal and connect with a VPN. Maybe this will change down the road.

I also want to make sure that this thing actually works even when we're across the country. Using a trusted camera and NVR is a good start. I'd also like to implement uptime monitoring and ensure everything is tested out before we spend time away to identify gaps.

To keep things practical, I also want to make sure I'm not having to buy endless TBs of storage to hold all the footage. I just don't have the money.

Lastly, some sort of Home Assistant integration would be great. I've seen some pretty cool setups with automations that involve cameras. I'd like to play with this down the road as well.

## The Plan

Very quickly in my search I came across [Frigate NVR](https://frigate.video/). An open-source network video recorder that ticks all the boxes and more. Everything works completely locally, it's a popular project relied on by many to record video, and it's got a lot of clever ways built in to only store the footage that matters. Last, but not least, it's got a very strong Home Assistant integration.

To keep things simple, I'll start with a single camera recommended to me by a friend, the [Reolink E1 Pro](https://www.amazon.com/dp/B08RS4C67L). This supports RTSP which allows it to connect to Frigate and outputs 2 video streams. One stream is the original quality, full-res stream, the other is a low-res copy of the first.

This low-res stream can be routed to Frigate's AI object detection engine with frame rates as low as 5 fps. This will give my humble HP MP9 G4 breathing room to run image recognition continuously. The object detection engine can be configured to trigger a video recording when a target from a preset list is seen. Once the target is seen, the full-res video stream is recorded. The NVR also has retention settings to prevent this bank of recordings from growing too large.

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
    D --> E
</div>

From there, Home Assistant has the ability to display live feeds, recorded videos, and use detection data to trigger automations. That's the plan for now. I'll keep things updated with progress.
