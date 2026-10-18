---
title: "Offline Media Server"
date: 2026-07-15
category: "Services: Organization"
excerpt: "A place to watch all of our movies"
header:
  teaser: /assets/images/jellyfin-teaser.png
tech_stack: [Proxmox, Ubuntu Server, Jellyfin]
---

https://jellyfin.org/docs/general/installation/linux/

## Stack

This server is running on Ubuntu server housed in it's own CT on Proxmox.

## What it does

Jellyfin is a media server you can use to organize and play back video based content such as TV Shows and Movies on other devices. It's pretty much like having your own digital streaming service with all of the media you own.

## Installation

Start by spinning up a new CT in Proxmox by clicking the new CT Button and following the steps in the wizard. I gave mine 4GiB of mem and swap as well as 3 CPU cores. This should be plenty of firepower to live decode media to 1 or 2 devices, which all I expect to need for the time being. From there you can install your OS, in this case I used Ubuntu Server 

As always, make sure you check for updates and upgrades once the OS is up and running.

'''
sudo apt update && upgrade -y
reboot
'''

From there, download the official install script and verify on system.

'''
curl -s https://repo.jellyfin.org/install-debuntu.sh -O && \
curl -s https://repo.jellyfin.org/install-debuntu.sh.sha256sum -O && \
sha256sum -c install-debuntu.sh.sha256sum
'''

Then run the install script. It will guide you through the install process.

'''
sudo bash install-debuntu.sh
'''

You have succesfully installed Jellyfin. It's really that easy.

## Setup

From there, you can point a web browser to your port 8096 on your new container (ie: 192.168.0.X:8096) where you should find the setup wizard is ready to go. This will take you through setting up your new server's language and creating an admin account. Be sure to save your login credentials.

From there you can add media libraries. I setup two folders to house media and added them on this step.

'''
/home/Movies/
/home/Shows/
'''

Jellyfin reccomends seperating your media types into directories and then further sorting each movie or show into a sub-directories using a specific format: Media Title (Year). For example, the movie Inception gets places in /home/Movies/'Inception (2010)'/. Shows get further segmented by season. The Office Season 1 gets placed in /home/Shows/'The Office (2005)'/'Season 01'/.

Seperating everything like this can be time consuming to get started if you have a lot of media you're organizing, but it makes Jellyfin's metdata analysis much more accurate so I believe it's worth the effort.

There are also some specific permissions that Jellyfin requires. It needs read, write and execute permissions for each directory a media file is located in and read permissions for each file in that directory.


## Challenges

The actual install and setup process is extremely straight forward on this one and I didn't run into any issues there. Two issues did come up during the post install process though.

First, when scanning for media, nothing was found at all. This was caused by a lack of permissions for the directories my media is housed in. It tooka while of digging through forums to find what causes this. I've since lost the post that solved my issue, but giving execute permissions to all of the folders in my media directories execute fixed this right away.

Once everything discovered, I also ran into an error when trying to play my media for the first time: 'Playback failed due to a fatal player error.' I found this was due a lack of read permissions on the actual media files themselves. This is pretty obvious looking back, but this is something I didn't think about initially so I figuired it's worth bringing up.

Once both of these were resolved, I was fully up and running and able to stream my media from any device.

## Next Steps

Something I want to improve in the future is how I manage my media. As of right now, I have two directories in /home that I sort my media into. I'd prefer to park everything on a remote drive and point Jellyfin to it. That way other services can use the media and I can manage it more easily.

## Resources

- (Jellyfin Docs - Installtion > Linux)[https://jellyfin.org/docs/general/installation/linux/]
- (Jellyfin Docs - Post-Install Setup)[https://jellyfin.org/docs/general/post-install/setup-wizard]
