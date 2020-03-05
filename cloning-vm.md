# Cloning Virtual Machine from its Base image
This short instruction that covers list of things needs to be considered or changed within the newly cloned VM before it can taken into use i.e to make sure it gets its own unique identity and do not mess up a production environment. 

I use these steps to prepare cloned Ubuntu Server 18.04 LTS based VM images (my current base image) for various personal projects and homelabs. I keep my base image is normally shutdown, except for updates, optimizations or rebasing. I use QNAP Virtualization Station (GUI interface) to make a VM clone from a base image and execute following steps in the cloned VM console or SSH to preprare VM for its actual use.

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
    
Optional steps
---
Check and resize virtual disc / lvm logical volume / filesystem according to application requirements. Inc I currently assign 32 GB virtual disk and out from that LVM seems to allocate 4 GB for '/' (>70% in-use).

    # Check volume and filesystem sizes
    $ lsblk
    $ df -h
    $ sudo lvdisplay -v /dev/ubuntu-vg/ubuntu-lv
    
    # Expanding virtual disc size will be added if needed
    
    # Increase LVM logical volume size (lvextend --size +<size-to-be-expanded> <lvm-logical-volume>, reference lsblk or lvdisplay)
    $ sudo lvextend --size +1G /dev/ubuntu-vg/ubuntu-lv

    # Increase filesystem size (resize2fs <filesystem>, reference df-h)
    $ sudo resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
    
    
References:
https://linuxhandbook.com/linux-list-disks/  
http://www.microhowto.info/howto/increase_the_size_of_an_lvm_logical_volume.html  
http://www.microhowto.info/howto/increase_the_size_of_an_ext2_ext3_or_ext4_filesystem.html
