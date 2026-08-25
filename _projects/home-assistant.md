---
title: "Smarthome Automations"
date: 2026-07-15
excerpt: "Complex automations that make day to life simpler"
header:
  teaser: /assets/images/homeassistant-teaser.png
tech_stack: [Proxmox, Ubuntu Server, Docker, Home Assistant]
---

## Overview

Home Assistant is a platform designed to connect smart devices and other additional data together. It allows you to trigger actions based on specific inputs or events. For instance, you can setup a button pressed on the app to turn on a smart bulb and change the color temperature based on the time of day. 

The sky's the limit and you can really take any set of inputs and conditions and then run a very complex set of actions. You're only limited by your hardware and your immagination.

## Stack

- Proxmox
- Docker / Docker Compose
- Home Assistant

## Additional Hardware

Home Assistant is pretty useless without hardware to control. Here's what I'm running:

### Hub

Unless you're using Wi-Fi compatible hardware, your hub will be what actually communicates with everything. I've seen alot of people mention that running a bunch of Wi-Fi comptible devices can greatly impact your network's performance. 

I chose a hub that is compatible with both Zigbee and Z-Wave protocols so that I don't get locked into one thing. Currently I've only got Zigbee hardware though.

- [Aeotec Z-Stick 10 Pro](https://www.amazon.com/Z-Stick-Pro-HomeAssistant-Zigbee2MQTT-Controller/dp/B0DV9RFSR9?crid=1C5UJEQ3ZBRHF&dib=eyJ2IjoiMSJ9.91cUVRCT7ubD_NiYa-NZ239XuIhUs8BK2hU7IYUwUK_ehJ0LiO71CFpBv-O9gK-3g-knp13T_i7nEYq30sGCghV1QV4S-Emqw_bsxtDbbp96A3ApmmZzOoJM3hRzJhwuTlhOCXa7HUjpyVuo6p1IYHQzo35HkA_bRrUC-G9CK3SIo6PjiuFWlVVFb_dJPJJJC0nEknW71alRtjepDBxG8vB18Hz-dYHTO0RdaTVHf-yhYWp0nwCS8dxeXqTIOEVN8SmtdOR-_AYV9KVkawLOIHK4Oj7zEt3oEAfnkSgclyQ.ZxTlBtlO8SlFW_T3ZX3d_-QJZ3vebur_NzHlkjNcA2A&dib_tag=se&keywords=aeotec+smart+home+hub&qid=1784584993&sprefix=aeote%2Caps%2C285&sr=8-5)

### Lighting

All the bulbs in my place are smart bulbs. Additionally, I've got a couple of string lights and smaller lamps that are controller with smart switches.

- 24x [Third Reality ZL1 RGBW Smart Bulbs](https://www.amazon.com/THIRDREALITY-required-Compatible-Assistant-SmartThings/dp/B0DHFSDV46?crid=338GCWD6M78YY&dib=eyJ2IjoiMSJ9.-F8adOfvGIs4BoJhNMKn5h_TaU5YQo1i-V8PyCbX5vdpGhpbyW6U1zS5h4xl8L5crC7qW2cZJcff1RUOVL6BXWEra1Aiz9MOKu7EvaXKy0f5UrbMlqdhrccUWvmiFEmVOkwfupqFLkeydQ3wp5DFqNceTWfIrcw2f1RVC1PvWTf4btcyrNmj_-W2bkY-H_oJrWrIkarhvF5jDIqtyaxWgw.kazs8U7SOOAu0dFHRdqO10fdeLk5u3SoFVKb8uXifPQ&dib_tag=se&keywords=rgb%2Blightbulbs%2Bthirdreality&qid=1784125764&sprefix=rgb%2Blightbulbs%2Bthirdreality%2Caps%2C192&sr=8-4&th=1)
- 4x [Third Reality Gen 2 Smart Outlets](https://www.amazon.com/THIRDREALITY-Real-time-Monitoring-Compatible-SmartThings/dp/B0BPY5D1KC?crid=1TQHX17JK5N8M&dib=eyJ2IjoiMSJ9.mSC0wmQk51fqfyTz0fcMFebhxuXFytBXl6OMLdKce9uuzABF1ECuYcD4J4SWQHxbTEbctYQ3QB511WHqCqI_N2KGiDBgl7px30GsMPUWI1eApiTLhPC0DweuRoGK6QUhK7uPPG9GbL4b8SYYXxhSfI0w68xbJ50KmzuQcTXGCYhKsIujG-Cx9-BxefaiDc5QVn6PqcIYM7gGJTFcS0dbuTS4KL1DOD4ubeGAdN4PttvOGg3pBK_k2MztApuWlEOjtLH_SR15G6nikfO1ViUtCAab4yFNtJehsGGVd7ktZ5w.PdyBQhiwvIvbnxddYhj6B33WjKtW1d4ltKPKiAZvGTM&dib_tag=se&keywords=third%2Breality%2Bsmart%2Bplug&qid=1784584950&sprefix=third%2Breali%2Caps%2C267&sr=8-6&th=1)

### External Input Devices

Buttons are handy remotes that allow you to manually trigger scripts without needing to pull out your phone. The motion sensors are even more automatic.

- 2x [Third Relity Zigbee Smart Buttons](https://www.amazon.com/dp/B0BTGXWXPR?ref=nb_sb_ss_w_as-reorder_k1_1_15&amp=&crid=TKF5XI9TAW6N&amp=&sprefix=smart+button+th)
- 3x [Aqara Zigbee 3.0 Motion Sensors](https://www.amazon.com/Aqara-Motion-Detector-Automations-Compatible/dp/B0FNWKMYHL?crid=1HZ4SI15RH6TG&dib=eyJ2IjoiMSJ9.amIBDZmCmanhE-yXqVNTWCt3bkzF2qqQ7hOvPNbvJ_wtDhpmKfhc9KsVm6VqzdKixgko2WAMictKLAXZ_t7-aJ3PIyhFvQxsF1RJpei2G9s-3d7FKQDOO0wMGOlEZcEltxXXJUNlNqT-hEzR3s69yKiVNTNox72MxYG4qtpDcrMEMzj9nFqRD04UO63HOONuX3Qc8v4whJ2kdbr3kOjcymt0tDxzoe80D9AHp--7xrNMvU-tZL3ACFp3Cmb2L2ilnh9i_nk3Kc1pmXaj1lCZTgVzX5qhbgIG7KHFX0UK4vs.hwkPjc5GXX-pfKK3le9yW2Bff4QlPLzlRaopBnv4z1M&dib_tag=se&keywords=aqara%2Bmotion%2Bsensor&qid=1784585086&s=electronics&sprefix=aqara%2Bmotion%2Bsenso%2Celectronics%2C232&sr=1-3&th=1)

## What it does

My smarthome is entirely focused around ensuring lighting happens as automatically as possible. Whenever I think about a new automation or change to an existing script, I think about it from one perspective: how can I make the lights match the mood without having to get up and flip a switch or pull out a phone and press a button. With this in mind, I've segmented everything based on time of day as follows:

- Early Morning
- Morning
- Daytime
- Evening
- Night

How a button press, motion sensor trigger, or other automation affects the lighting depends on the time of day.

## Setup

TODO

## Challenges

TODO

