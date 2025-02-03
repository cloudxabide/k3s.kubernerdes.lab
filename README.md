# k3s.kubernerdes.lab

## Purpose and Narrative

I created this repo to demonstrate how to:
* deploy k3s on NVIDIA Jetson Xavier NX (now featuring Ansible).
* deploy k3s on vSphere Virtual Machines (x86)

This repo is a *rather* opinionated approach in that I am using NVIDIA Jetson Xavier NX modules (with a specific disk layout that I have opted for) as well as Ansible to execute the set up of the devices.  That said, this is not a generic how-to on how to run K3s on NVIDIA Jetsons.

A number of the specific and opinionated items mentioned in this repo are actually quite flexible.  I have decided on a particular architecture and approach in order to make this a solution that absolutely works (tm), given that the guidance provided is followed.

### Status
2025-02-2 -- While I am trying to figure out how to deploy on the Jetsons, I will focus on deploying on vSphere, and then integrating with Rancher.

2025-01-21 -- Progress. It turns out that the steps I was using to confirm success, were not valid on the Jetson.  I.e. you cannot run nvidia-smi on a Jetson (it is for PC-based GPU)

2025-01-18 -- I still have yet to find the combination of Repos + Packages to make this work.  Resorting to using SDK to install Jetpack 4.6.1 and all the supporting software on to the NVMe device. :-(

2025-01-17 -- pre-release, still being built. 

## Prerequisites
* Fairly strong Linux background or knowledge.  And not because you will need to create anything with Linux skills, but moreso that you understand the steps/tasks called out in this repo.

* Basic familiarity with managing NVIDIA Jetson devices
  * How to set jumpers
  * How to connect to devices via USB in order to flash NVIDIA devices
  * How to flash the devic
* Moderate AWS knowledge and some appetite to incur costs (*)

## Bill of Materials
This is not an exhaustive list (at this time).  These are the more important/obvious items.

Physical Assets
* NVIDIA Jetson Xavier NX 
 * 3 x [Seeed Studio - reComputer J2011-Edge AI Device with Jetson Xavier NX 8GB module](https://www.seeedstudio.com/Jetson-20-1-H1-p-5328.html) with Power Supplies  
 * 5-port 1GB Network Switch and cables (4 x host + 1 x uplink)
 * System capable of running [Ubuntu 20.04 LTS](https://ubuntu.com/download/desktop) and [NVIDIA SDK Manager](https://developer.nvidia.com/sdk-manager) to flash the NVIDIA Jetson Xavier modules
* VMWare vSphere 
 * NUC13ANHi7 - Processor Type:	13th Gen Intel(R) Core(TM) i7-1360P, 64 GB Memory
 * 5-port 1GB Network Switch and cables (4 x host + 1 x uplink)
* Common 
 * Keyboard/Video/Mouse  
* Internet Connectivity from Jetsons (while air-gapped is possible, it is well out of the scope here)

Software, etc..
* NVIDIA Jetson Xavier NX Software (acquired via NVIDIA SDK Manager)
* SUSE Rancher 
* SUSE K3s
* Amazon account with available quota run this workload
* VPN connectivity to AWS Transit Gateway

![High Level Overview](./Images/High_Level_Overview.drawio.png)

## References and Notes
Some of this code brought to you by [Amazon Bedrock](https://aws.amazon.com/bedrock/) - specifically using [chat-cli](https://github.com/chat-cli/chat-cli) (Which I highly recommend - it's quite simple to get started)

[AI at the Edge with K3s and NVIDIA Jetson Nano: Object Detection and Real-Time Video Analytics](https://www.suse.com/c/ai-at-the-edge-with-k3s-nvidia-jetson-nano-object-detection-real-time-video-analytics-src/)  

[Run an Edge AI K3s Cluster on NVIDIA Jetson Nano Boards](https://www.suse.com/c/running-edge-artificial-intelligence-k3s-cluster-with-nvidia-jetson-nano-boards-src/)

[NVIDIA Github - Docker won't run correctly](https://github.com/dusty-nv/jetson-containers/issues/108) - a dustynv thread
(*) NOTE:  You can use this repo to deploy just a K3s cluster on the NVIDIA Jetsons (and not worry about Rancher and the subsequent AWS cost.)
