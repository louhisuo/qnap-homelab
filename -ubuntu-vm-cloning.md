Virtualization in QNAP allows cloning of VM through its GUI. Following things needs to be reconfigured / cleaned within the newly cloned VM before taking it to use. This is to ensure that it will get its own unique identidy and not get mixed with base image VM. Use console access to execute these commands as you will lose remote connection while doing changes.

Change hostname within the cloned VM  
Change hostname within /etc/hostname  
$ hostnamectl status  
$ sudo hostnamectl set-hostname <new-hostname>  

Change hostname within /etc/hosts (for a local host entry 127.0.1.1)  
$ sudo nano /etc/hosts  

# check also /etc/cloud/cloud.cfg  

Flush IP address (and MAC addresses) within the cloned VM  
$ sudo ip address flush scope global  
$ sudo dhclient -v  

Reinitialize SSH host keys
$ sudo rm /etc/ssh/ssh_host_*  
$ sudo dpkg-reconfigure openssh-server  

Reboot the cloned VM  
$ sudo reboot  

---

For future study / cleanup, check following ...   
# on older versions of Ubuntu and RHEL variants  
$ rm /etc/udev/rules.d/70-persistent-net.rules  
$ sudo vi /etc/network/interfaces  

# on newer versions of Ubuntu with systemd networking  
# remove cloned ids used as DHCP identifier  
sudo rm /etc/machine-id /var/lib/dbus/machine-id   
# recreate ids unique to guest  
sudo dbus-uuidgen --ensure && sudo cp /var/lib/dbus/machine-id /etc/.  
