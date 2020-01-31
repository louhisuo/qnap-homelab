# Building Ubuntu Server base image Virtual Machine (VM)
Rough guide to build Ubuntu 18.04 LTS server based VM base image for various homelab purposes. Homelabs are running on QNAP Virtualization Station 3 based on QTS 4.4.1 firmware.

Create VM (Ubuntu Server)
---
Download Ubuntu server iso-image from https://ubuntu.com/download/server

Create VM by using QNAP Virtualization Station. In 'Create VM' window use following settings  
- CPU Cores: 2  
- Memory: 2 GB  
- HDD Storage: 32 GB  

During installation, pay attention to following  
- Disc partitioning: Choose entire disc and use LVM  

Set hostname for the base image VM (/etc/hostname and /etc/hosts)

    $ hostnamectl
    $ sudo hostnamectl set-hostname <new-hostname>
    $ sudo sed -i 's/\b<old-hostname>\b/<new-hostname>/g' /etc/hosts
    or
    $ sudo nano /etc/hosts

Make hostname change persistant by changing 'preserve_hostname' from 'false' to 'true'  

    $ sudo nano /etc/cloud/cloud.cfg

Reboot

    $ sudo reboot

Update packages within VM

    $ sudo apt update
    $ sudo apt list --upgradable
    $ sudo apt upgrade
    $ sudo apt auto-remove

Install VM helper tools (qemu-guest-agent) to make it possible for host to communicate with guest VM e.g. to obtain IP address and show it in QNAP Virtualization Station.
    
    $ sudo apt install qemu-guest-agent

Print Machine ID (to be used as an DHCP Client Identifier) and use it to configure an entry in DHCP server.

    $ hostnamectl
    or
    $ cat /etc/machine-id
    

Reboot (to finalize base image configuration)

    $ sudo reboot

Create VM (Ubuntu Minimal cloud image)
---

References:  
https://ubuntu.com/download/server  
https://wiki.ubuntu.com/Minimal  
http://cloud-images.ubuntu.com/minimal/releases/bionic/release/  
https://www.howtoforge.com/tutorial/ubuntu-lts-minimal-server/  
https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-change-an-ubuntu-18-04-server-hostname/  
https://help.ubuntu.com/community/RootSudo  
https://wiki.debian.org/MachineId  
https://wiki.qemu.org/Features/GuestAgent


TO-DO:
Build downscaled, cloud optimized base image from Ubuntu Minimal (https://wiki.ubuntu.com/Minimal).
    
