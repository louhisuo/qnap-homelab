# Things to consider when cloning Ubuntu Virtual Machine from its base image
This short instruction that covers list of things needs to be changed within the newly cloned VM before it can taken into use i.e to make sure it gets its own unique identity and do not mess up a production environment. 

I use these steps to prepare cloned Ubuntu Server 18.04 LTS based VM images (my current base image) for various homelabs. My base image is normally shutdown, except for updates or base image optimizations. I use QNAP Virtualization Station GUI to make a VM clone from a base image and execute following steps in the cloned VM console before installing any applications into the cloned VM.

If you clone your production VM, just make sure that you keep it paused or shutdown until you have executed steps below otherwise you may experience hostname, IP address, MAC address duplications and SSH troubles.
    
Change hostname within a cloned VM '/etc/hostname' file

    $ hostnamectl status
    $ sudo hostnamectl set-hostname <new-hostname>
    
Change hostname within a cloned VM '/etc/hosts' file and also check '/etc/cloud/cloud.cfg' for its hostname setting.

    $ sed -i 's/\b<old-name>\b/<new-name>/g' /etc/hosts

or alternatively make a change with your favorite text editor

    $ sudo nano /etc/hosts

Generate new Machine ID for cloned VM

    $ sudo rm /etc/machine-id 
    $ sudo rm /var/lib/dbus/machine-id
    $ sudo dbus-uuidgen --ensure
    $ sudo cp /var/lib/dbus/machine-id /etc/.

Check that product_uuid / system-id is unique for the cloned VM (it is also visible as UUID in QNAP Virtualization Station GUI). 
Every VM should have unique UUID, otherwise something unexpected may happen.

    $ sudo cat /sys/class/dmi/id/product_uuid
    $ sudo dmidecode -i system-id

Reinitialize SSH host keys.

    $ sudo rm /etc/ssh/ssh_host_*
    $ sudo dpkg-reconfigure openssh-server
    
Flush IP address (and MAC address) within the cloned VM.

    $ sudo ip address flush scope global
    $ sudo dhclient -v

Check that cloned VM has an unque IP and MAC address

    $ sudo ip address
    $ sudo ip link

Reboot the cloned VM

    $ sudo reboot
    
---

References:
