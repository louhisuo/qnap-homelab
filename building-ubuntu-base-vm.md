# Building Ubuntu Server base image Virtual Machine (VM)
Rough guide to build Ubuntu 18.04 LTS server based VM base image for various homelab purposes. Homelabs are running on QNAP Virtualization Station 3 based on QTS 4.4.1 firmware.

Create a new VM
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

Update 

References:  
https://ubuntu.com/download/server  
https://wiki.ubuntu.com/Minimal  
http://cloud-images.ubuntu.com/minimal/releases/bionic/release/  
https://www.howtoforge.com/tutorial/ubuntu-lts-minimal-server/  
https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-change-an-ubuntu-18-04-server-hostname/  
https://help.ubuntu.com/community/RootSudo  
https://wiki.debian.org/MachineId  

TO-DO:
Build downscaled, cloud optimized base image from Ubuntu Minimal (https://wiki.ubuntu.com/Minimal).
    
