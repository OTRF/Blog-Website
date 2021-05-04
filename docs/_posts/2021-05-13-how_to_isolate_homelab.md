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


## What to Expect from this Post:
My aim for this post, and ideally for a continued series, is to provide a simple straight forward approach to setting up a malware analysis lab. The best part is that nearly all the tools I will be using are open source or have an open source alternative, meaning there isn't any cost to get started.  Only expense will be a physical machine to host several VMs at one time. I'm hoping this will help out others, while also reinforcing old concepts and learning new ones for myself.   

## Before We Start:
* I will be using VMware Fusion Pro for this walkthrough.  I have had the best experience by far with VMWare's line of virtualization software.  However, VirtualBox can be a great, free, substitute for VMWare.
* Troubleshooting the installation of virtualization software and/or the individual VMs is out-of-scope for this post.  There are just too many things that might go wrong.  If you do run into trouble, Google is your best friend.  
* When you run multiple virtual machines(VMs) on a single host machine, the host machine will slow down.  Because of this, it is important to give each VM its recommended settings for optimal performance.  For Windows 10, I recommend at least 2 processor cores and 4GBs of RAM.  For REMnux, 1 processor cores and 2GBs of RAM.

## Pre-requisites
* `VMWare Fusion(MAC)/ Workstation(Windows/Linux)`: VMWare has some great, comprehensive guides to install both [Fusion](https://kb.vmware.com/s/article/2014097) and [Workstation](https://kb.vmware.com/s/article/2057907).  VMWare does offer trial licenses for those interested in trying out the full feature set VMWare Pro line(Fusion Pro and Workstation Pro). VMware also has its Player line, which is free for personal use.  Only downside is that the Player version doesn't allow network customization that you should use for your lab.  Additionally, only Fusion Player has the ability to take snapshots.  Which is the major difference between Workstation Player and Fusion Player.  Hopefully VMware fixes that in the future.
* `VirtualBox`: Is the free alternative to VMware and some of the other virtualization software out there.  It also has all the feature you need in a VM solution starting out.  You can get a copy of VirtualBox [here](https://www.virtualbox.org/).
* `Windows Edge Developer ISO`: You can download a Windows ISO file: [here](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/).  We will be doing this later in the post.
* `FLARE VM`: FLARE VM is free malware analysis VM with a ton of tools and features pre-installed by FireEye. Its a great addition to your malware analysis toolset.  You can find instructions to install it [here](https://www.fireeye.com/blog/threat-research/2018/11/flare-vm-update.html).   
* `REMnux`: REMnux is a powerful Linux VM that has a great collection of tools for Malware Analysis by Lenny Zeltzer [here](https://remnux.org/).  You can find a lot of helpful reasources on his site including REMnux and reversing cheatsheets as well as blog posts that you might find useful.