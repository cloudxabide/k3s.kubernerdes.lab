# k3s.kubernerdes.lab

## Purpose
How to deploy k3s on NVIDIA Jetson Xavier NX (now featuring Ansible).

NOTE: This repo is a *rather* opinionated approach in that I am using NVIDIA Jetson Xavier NX modules (with a specific disk layout that I have opted for) as well as Ansible to execute the set up of the devices.  That said, this is not a generic how-to on how to run K3s on NVIDIA Jetsons.

### Status
2025-01-17 - pre-release, still being built

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
* 3 x [Seeed Studio - reComputer J2011-Edge AI Device with Jetson Xavier NX 8GB module](https://www.seeedstudio.com/Jetson-20-1-H1-p-5328.html) with Power Supplies  
* 5-port 1GB Network Switch and cables (4 x host + 1 x uplink)
* Keyboard/Video/Mouse  
* System capable of running [Ubuntu 20.04 LTS](https://ubuntu.com/download/desktop) and [NVIDIA SDK Manager](https://developer.nvidia.com/sdk-manager) to flash the NVIDIA Jetson Xavier modules
* Internet Connectivity from Jetsons (while air-gapped is possible, it is well out of the scope here)

Software, etc..
* NVIDIA Jetson Xavier NX Software (acquired via NVIDIA SDK Manager)
* SUSE Rancher 
* SUSE K3s
* Amazon account with avialable quota run this workload
* VPN connectivity to AWS Transit Gateway

![High Level Overview](./Images/High_Level_Overview.drawio.png)

## Narrative
A number of the specific and opinionated items mentioned in this repo are actually quite flexible.  I have decided on a particular architecture and approach in order to make this a solution that absolutely works (tm), given that the guidance provided is followed.

## References and Notes
Some of this code brought to you by [Amazon Bedrock](https://aws.amazon.com/bedrock/) - specifically using [chat-cli](https://github.com/chat-cli/chat-cli) (Which I highly recommend - it's quite simple to get started)

(*) NOTE:  You can use this repo to deploy just a K3s cluster on the NVIDIA Jetsons (and not worry about Rancher and the subsequent AWS cost.)
