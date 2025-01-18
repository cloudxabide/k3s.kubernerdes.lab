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


