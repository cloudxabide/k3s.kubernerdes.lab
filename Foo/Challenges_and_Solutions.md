# Challenges and Solutions

## Storage constraints
The NVIDIA Jetson Xavier NX modules I got from Seeed Studio are equipped with 2 x storage devices - SD Card (16GB) and NVMe (128GB).
As the NVMe device is more performant than the SD card, I opted to install the OS on the SD card and use the NVMe for application storage.

### Challenge 1
When you use SDK manager to install the CUDA and other supporting libs, it will test the capacity availble in nvidia's home directory (/home/nvidia) which is located in / (by default)

I created an Ansible playbook to create an PV, VG, and LVs on the NVMe device, and then carved out a volume for /home (lv_home).
I had another playbook attempt to swap out the /home (from /) with lv_home - and Ansible would FAIL at that point as Ansible tmp artifacts would no longer be available (~/.ansbile/tmp/ansible-tmp-(randomn UUID)).  I attempted to run the same playbook using: --extra-vars "ansible_remote_tmp=/tmp/.ansible/tmp"

#### Solution
I have decided to create a seperate Ansible User "My Ansible" (mansible) using /var/mansible as the home directory - which is actually a better approach.

## Supporting Infrastructure

### Challenge 1 - Ubuntu 
To utilize the NVIDIA SDK Manager to flash the devices, you need Ubuntu 20.04.  The "compatibility matrix" between devices and supporting OS is quite complext.  I believe there are CLI methods to flash the devices, also.  At this time, my efforts are better used figuring out other stuff.

#### Solution
I have found that using a KMV-based VM is acceptable.  There *are* some quirks, but no show-stoppers.

### Challenge 2 - Can't install software using SDK Manager
After the initial flash of L4T on to the Xavier, you need to install the supporting software (CUDA, etc...) It seems to take forever installing over ethernet and therefore the USB option seems ideal.

#### Solution
It takes a few minutes between when the Xavier has completed the L4T install, and when it will be available again to install via USB.  Just start the process in SDK Manager and let it fail and retry - my experience has been that it *will* eventually start to work and present you with the "Install" button to click through.

### Challenge 3 - Ubuntu version matters
It seems that each version of Ubuntu supports a different L4T release.  The process of installing L4T appears to rely on the host filesystem for bits.  I find this challenge fairly disappointing - I have managed to get this working using a VM, but I don't want to have several VMs to maintain simply to try out different versions of JetPack.  Additionally, they do not all operate the same - I am fairly certain I cannot get k3s working on the L4T that is created using Ubuntu 20.04 

| Jetson Linux(L4T) Version |	JetPack Version | Ubuntu Version |
|:----|:----|:------|
| | JP-5.1.4 | 18.04  |
| | | 20.04 |
| | | 22.04 |

Source: [NVIDIA JetPack Archive](https://developer.nvidia.com/embedded/jetpack-archive)

