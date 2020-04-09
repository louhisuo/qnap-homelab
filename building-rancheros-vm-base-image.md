# Building RancherOS VM Base Image
This guide is work in progress and is not really useful, expect to setup a single machine playground for Docker "powered by RancherOS".

Download an RancherOS image from (I am currently using rancheros.iso)

    https://github.com/rancher/os/blob/master/README.md

Create VM by using QNAP Virtualization Station. In 'Create VM' window use following settings

    - OS type: Generic
    - Memory: 2048 MB (note: with 1024 MB) VM will not boot up)
    - CD image: rancheros.iso
    - HDD storage: 8 GB
    - Network: Connect to: <choose virtual switch defined earlier>

Create and start VM to boot RancherOS from ISO image. Open VM console and confirm that RancherOS is up and running.   
YOU HAVE NOW WORKING RANCHEROS PLAYGROUND READY (... with some cavenets which I plan to address.


References:
- https://rancher.com/rancher-os/  
- https://rancher.com/docs/os/v1.x/en/overview/  
- https://github.com/rancher/os/blob/master/README.md  
- https://linuxhint.com/install_rancher_os/  
