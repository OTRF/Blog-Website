---
layout: post
current: post
cover:  assets/images/blog/how_to_isolate_homelab_images/2021-05-13_00_cover_image.jpg
navigation: True
title: Malware Analysis Series - Part 2, How to Isolate our Homelab with Network Segmentation
date: 2021-05-13 12:00:00
tags: [VMware, Malware, Malware_Analysis, REMnux, FLARE]
class: post-template
subclass: 'post'
author: joshua
---

# Introduction:

In part one of this series, we established a solid foundation to begin our malware analysis journey.  We successfully stood up two VMs; a Windows(FLARE) machine and a Linux(REMnux) machine. Put them on their own isolated virtual network without access to the internet. Lastly, we configured FLARE to us REMnux as its Gateway and DNS so that we could monitor its network communications.  We tested this by setting up INetSim on REMnux and trying to connect to a "malicious" site on our FLARE VM.

In part 2, we will be looking at isolating our home lab machine from the rest of our network through network segmentation.  Network segmentation is a critical component of secure network architecture.  It is a way of dividing a network into various segments, usually referred as subnets, that act as their own smaller network.  This offers several benefits like more easier control over the flow of traffic between different subnets through the use of policies and enhancing security.  Businesses and enterprises have used network segmentation for years but can be just as beneficial to home networks.  These benefits can extend way beyond our malware analysis series as well.

You might be thinking, “Why do I need to network segmentation when I  already have my VMs on an isolated VM network?”.  The honest answer is you don’t.  You can still use the setup we created in part 1 to analyze all kinds of malicious samples and be adequately protected from them.  However, it isn’t good practice to have your home lab machine and VMs on the same network and able to communicate with the rest of your devices.   There are several reasons for this:

1. You will need to get your file samples from somewhere and normally you will be grabbing samples from public repositories like VirusTotal.  Doing so on a machine that shares the same network as the rest of your devices puts them at risk of infection if you accidentally detonate the file on your physical machine.
2. In some instances you might want to provide your malware sample with a live internet connection.  Possibly as a way of getting a secondary file from a downloader you’re analyzing.  If your home lab machine is on the same network it opens up possible routes for the malware to spread itself.  
3. In extremely rare cases, malware might be able to exploit a vulnerability and ‘escape’ the VM.  If this happens, the malware will be able to infect your actual machine and possibly the rest of your network.  

## Before we get started:


