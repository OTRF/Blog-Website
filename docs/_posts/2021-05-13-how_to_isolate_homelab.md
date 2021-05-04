---
layout: post
current: post
cover:  assets/images/blog/how_to_isolate_homelab_images/2021-05-13_00_cover_image.jpg
navigation: True
title: Malware Analysis Series - Part 2, How to Isolate our Homelab from the Rest of our Network
date: 2021-05-13 12:00:00
tags: [VMware, Malware, Malware_Analysis, REMnux, FLARE]
class: post-template
subclass: 'post'
author: joshua
---

In part one of these series, we were able establish a solid foundation to begin our malware analysis journey.  We successfully stood up two VMs; a Windows(FLARE) machine and a Linux(REMnux) machine. Put them on their own isolated virtual network without access to the internet. Lastly, we configured FLARE to us Remnux as its Gateway and DNS so that we could monitor its network communications.  We tested this by setting up INetSim on REMnux and trying to connect to a "malicious" site on our FLARE VM.
