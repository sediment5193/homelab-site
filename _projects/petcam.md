---
title: "Smart Petcam"
date: 2026-07-15
category: "Services: Day to Day"
excerpt: "Local, AI powered, network video recorder"
header:
  teaser: /assets/images/frigate-teaser.png
tech_stack: [Proxmox, Ubuntu Server, Docker, Frigate NVR]
---

## Overview

Frigate is an open source network video recording software designed to power AI powered object detection on local hardware. The camera feed never leaves the network so it's totally private and doesn't rely on internet access. It also integrates smoothly into home assistant, letting you power more advanced automations.

## Stack

- Proxmox
- Docker / Docker Compose
- Frigate NVR

## Additional Hardware

- Webcam

## What it does

## Setup

- Docker compose
- Frigate YAML
- MQTT
https://github.com/sukesh-ak/setup-mosquitto-with-docker
https://www.simplepush.io/blog/frigate-nvr-push-notification-guide
https://github.com/HenkVanHoek/frigate-ha-docker-compose
- HA Integration

## Challenges


- Double pass iGPU pass-through 
- Dialing in settings
