# qnap-ubuntu-server
A brief guide how to create usable Ubuntu server base image on QNAP Virtualization Station for various projects.
Ubuntu 18.04 LTS on QNAP firmware release 4.4.1 w/ Virtualization Station 3.

---
Download Ubuntu Server iso image from https://ubuntu.com/download/server and create a VM with Virtualization Station 3.  
Note: Check Ubuntu Minimal (https://wiki.ubuntu.com/Minimal) in future. 

---
Update packages within VM  
$ sudo apt update  
$ sudo apt upgrade

---
Check that SSH packagge was installed and SSH daeemon is running  
$ apt show ssh  
$ systemctl status sshd

---
If needed install SSH package to enable remote CLI access 
$ apt install ssh

---
Install VM helper tools (qemu-guest-agent) to able host to communicate with guest VM.
"The QEMU guest agent is a daemon that runs on the virtual machine. The agent passes network information on the virtual machine, notably the IP address of additional networks, to the host."
For more information, see https://wiki.qemu.org/Features/GuestAgent

$ sudo apt install qemu-guest-agent
