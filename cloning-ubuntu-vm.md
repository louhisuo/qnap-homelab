# Cloning Ubuntu VM from a base image
This short instruction that covers list of things needs to be changed within the newly cloned VM before it can taken into use i.e to make sure it gets its own unique identity and do not mess up a production environment. I use these steps to prepare cloned Ubuntu Server 18.04 LTS based VM images (my current base image) for various homelabs. My base image is normally shutdown, except for updates or base image optimizations. I use QNAP Virtualization Station GUI to make a VM clone from a base image and execute following steps before installing any applications into the cloned VM. If you clone your production VM, just make sure that you keep it paused or shutdown until you have executed steps below otherwise you may experience hostname, IP address, MAC address duplications and SSH troubles.

Change hostname within a cloned VM '/etc/hostname' file

    $ hostnamectl status
    $ sudo hostnamectl set-hostname <new-hostname>
    
    
