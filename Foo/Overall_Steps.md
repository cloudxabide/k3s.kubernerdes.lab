# Overall Steps

## Status
This doc is a work in progress.  I hope to have this as an outline/roadmap to follow once I know what all the steps are.  Many docs I have found are either outdated, or a bit "hand-wavy"

## Physical Steps and Flash Jetson
Build Ubuntu host for running SDK manager
Jumper pins on NVIDIA Jetson Xavier NX module to force restore

## SDK Manager Tasks
Using SDK Manager, flash the host Jetpack 5.1.4, but only the Linux for Tegra (L4T) and not the Jetson Runtime Components

## Shell tasks
-- add ssh key (for nvidia user)
-- add mansible user (My Ansible) with home dir outside of /home

## Ansible Tasks
- configure power mode
- enable sudo
- create new volumes/mounts

## SDK Manager Tasks
- Install Jetson Runtime Components using Ethernet (USB might also work - need to test this)

## ???
- Still trying to figure out what next steps are
