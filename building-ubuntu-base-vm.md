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

Update, Install and Remove packages
---
Update packages within VM to the latest versions

    $ sudo apt update
    $ sudo apt list --upgradable
    $ sudo apt upgrade

Install VM helper tools (qemu-guest-agent) to make it possible for host to communicate with guest VM e.g. to obtain IP address and show it in QNAP Virtualization Station.  
TO-DO: Study if anything needs to be installed from QNAP guest-tools.iso. It is distributed as part of Virtualization Station package for QNAP and its location can be found within QNAP with command 'find / -name guest_tools.iso'
    
    $ sudo apt install qemu-guest-agent

Print a system boot-up statistic and optimize boot-up time

    $ systemd-analyze blame
    

Clean up unused packages

    $ sudo apt auto-remove

Finalize the VM base image
---
Print MAC address, to be used to configure fixed IP address mapping in DHCP server.  
TO-DO: Study how DUID is used as dhcp-client-identifier / Client-ID in DHCP queries and is there a convinient command to print it. Machine-ID which can be printed using 'hostnamectl' or 'cat /etc/machine-id' seems to be used to construct DUID.

    $ ip address    

Reboot (to finalize base image configuration)

    $ sudo reboot



References:  
https://ubuntu.com/download/server  
https://wiki.ubuntu.com/Minimal  
http://cloud-images.ubuntu.com/minimal/releases/bionic/release/  
https://www.howtoforge.com/tutorial/ubuntu-lts-minimal-server/  
https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-change-an-ubuntu-18-04-server-hostname/  
https://help.ubuntu.com/community/RootSudo  
https://wiki.debian.org/MachineId  
https://wiki.qemu.org/Features/GuestAgent  

Create VM (Ubuntu Minimal cloud image)
---
Download Ubuntu Minimal 18.04 LTS image from (choose .img file)

    http://cloud-images.ubuntu.com/minimal/releases/bionic/release/

Create VM by using QNAP Virtualization Station. In 'Create VM' window use following settings  
- CPU Cores: 2  
- Memory: 2 GB  
- HDD Storage: 32 GB  
- HDD Location: Choose 'Use existing image' and point location to downloaded .img file.
- Network: Choose virtual switch available.

Parked for timebeing. Deploying Ubuntu Minimal requires converting .img to able to use it within QNAP Virtualization Station.

References:  
https://wiki.ubuntu.com/Minimal  
http://cloud-images.ubuntu.com/minimal/  

TO-DO:
---
Build downscaled, cloud optimized base image from Ubuntu Minimal (https://wiki.ubuntu.com/Minimal).
    
