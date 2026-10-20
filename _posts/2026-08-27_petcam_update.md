---
title: "Pet cam update"
date: 2026-08-27
categories: [networking, physical-security]
tags: [docker, home-assistant, frigate]
excerpt: "Why buy a security camera when a webcam does the trick?"
---

## Overview

This is a follow up to my [previous post which you can read about here to get up to speed.](/homelab-site/posts/2026/07/27/localized-pet-cam/) My buddy read my previous post and had a great suggestion. He was concerned about how my hardware would handle real time AI image processing and had the idea of testing the idea out with existing hardward. After some thought, I pulled out an old USB webcam and thought longingly about how simple my life was before I started homelabbing.

Using a wired webcam has the obvious limitation of reducing where I can place this camera to about 5' or 6' from my server. Fortunately my servers are in the living room which is exactly where I wanted to place the camera anyway. So that works out.

## Setup

As I mentioned in my last post, I'll be using Frigate to run all of this. Frigate warns against running the service in any sort of VM and highly reccomends running everything on bare metal. This is to simplify device management and maximize performance. So, I of course decided to install frigate in a Docker container running in a Proxmox container. Genius I know.

This post will focus more on the specifics of getting a webcam up and running in Frigate. [More details about the install and setup can be found on the dedicated project page here.](/homelab-site/projects/petcam/)

Once frigate is up and running, you need to setup a "double pass iGPU pass-through". Essentially passing the USB device and video driver through from the bare metal, to the Proxmox CT, then through Docker to Frigate. It sounds complicated, but it's not as bad as you'd think. Start by finding the IDs of everythign you're passing through.

Open a shell directly on the node Frigate is running on and your webcam is plugged into. Then mavigate to /dev and search for video devices.

<pre>
    <code>
        cd /dev
        ls | grep video
    </code>
</pre>

Most people should just get 2 results from the 2nd command: video0 and video1, and will need to jsut pass through those two results. However, this returned a few results since my old laptop I'm running this on also has a built in webcam. I ultimatelly passed through video2 and video3.

This was an educated guess. I figuired that since the built in webcam was attached first, it'd probably be assigned video0 and video1 which left video2 and video3 for the USB webcam. This wound up working out. You'll also want to locate the device name of your GPU. Navigate to /dev/dri and list the directory contents.

<pre>
    <code>
        cd /dri
        ls
    </code>
</pre>

I only had one device listed: renderD128, so this was an easy decision.

Once I decided on what I'd be passing through, I opened the resources tab on the container that I'll be passing these to. To add each device I clicked Add > Device Passthrough and added each device's respecitve path: /dev/video[ID]. I left all of the other fields as default.

Then I added my laptop's iGPU by adding: /dev/dri/renderD128 using same add button. I also rebooted the container to ensure everything is in place.

Now that we've got everything passed through to the Proxmox container, we need to figuire out which video formats are supported by your webcam and which of the webcam's many streams to use. There's a helpful utility that will help us list all the options out. Navigate to your container's console and install the following:

<pre>
    <code>
        apt install v4l-utils
    </code>
</pre>

Then to list everything out, you'll want to run the next command. Be sure to plug in the ID of the first video device you passed through.

<pre>
    <code>
        v4l2-ctl --list-formats-ext -d /dev/video2
    </code>
</pre>

This also tells us whether the device is passed through properly since any issues up to this point would result in an error. Next you'll want to modify the docker compose file to setup the final passthrough for our GPU and also enable hardware acceleration. Open your Docker compose:

<pre>
    <code>
        cd ~/Frigate && nano docker-compose.yml
    </code>
</pre>

Ensure the following is added to your Docker compose file:

<pre>
    <code>
        devices:
            - /dev/dri/renderD128:/dev/dri/renderD128
            - /dev/accel:/dev/accel # Intel NPU
    </code>
</pre>

Then exit nano and rebuild:

<pre>
    <code>
        docker compose up -d
    </code>
</pre>

Once Frigate is fired up, it should have access to everything it needs to get this camera going. Open the service in your browser and navigate to the Config Editor. I added the following to my config file:

<pre>
    <code>
        detectors:
            ov_0:
                type: openvino
                device: GPU

        model:
            width: 300
            height: 300
            input_tensor: nhwc
            input_pixel_format: bgr
            path: /openvino-model/ssdlite_mobilenet_v2.xml
            labelmap_path: /openvino-model/coco_91cl_bkgr.txt

        record:
            enabled: true
            alerts:
                retain:
                    days: 3
                    mode: motion
        detections:
            retain:
                days: 3
                mode: motion

        cameras:
            webcam:
                enabled: true
                ffmpeg:
                inputs:
                    - path: /dev/video2
                    input_args: -f v4l2 -input_format h264 -video_size 1280x720 -framerate 15
                roles:
                    - detect
                    - record
                detect:
                    enabled: true
                    width: 864
                    height: 480
                    fps: 5

                    objects:
                        track:
                            - cat
        
                motion:
                    threshold: 30
                    contour_area: 30
                    improve_contrast: false

                record:
                    enabled: true

                snapshots:
                    enabled: true
                    timestamp: false
                    bounding_box: false
                    crop: false
                    retain:
                        default: 14
    </code>
</pre>

This results in the webcam only recording when it detects a cat. The recordings are retained for 3 days and then automatically deleted. I decided on an input reoslution of 720p at 15 fps. I'm then downsampling that to 480p at 5 fps for the detection portion of the process. 

This downsampling procoess causes a bit of a CPU hit, but massively frees up the GPU for detections. It also limits the distance from the camera that an object can be detected reliably, but I'm recording a cat in a 1 bedroom apartment so it's really not an issue for me. Once a detection happens, I'm then recording at the full input resolution.

When it's all said and done, I'm avergaing about 10% to 15% on the CPU and GPU with an inference speed of less than 15ms. I could further improve these numbers by adding an external TPU to my system, but I'm fine with this and TPU's aren't free.

So far everything is working very smoothly and detections are very reliable. Like many webcams, there's poor low-light performance and a lack of PTZ. I really just want to make sure my cat is still kicking it while I'm out of town. So I don't think I'll need to buy an extra camera or TPU after all.

## Next Steps

First I'd like to measure how much storage space the recordings actually take. With the automated retention policy, I feel like I can dial that in pretty nicely.

Then I'd like to get this tied in with Home Assistant for smarter notifications and all the automations. I'll need to setup an MQTT broker to get that going. I'll follow up with more on that once it's working.

I'd also potentially like to add a doorbell camera since we've got so much headroom with the current setup. That's not a super high priority for now and will ultimately depend on how much storage space the webcam needs.

## Resources

- https://docs.frigate.video/frigate/camera_setup/
- https://www.xda-developers.com/frigate-turned-old-usb-webcam-into-self-hosted-security-camera-motion-detection/
