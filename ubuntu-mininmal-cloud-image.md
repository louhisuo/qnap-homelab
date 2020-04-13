# Building Ubuntu Minimal Server Cloud Image
My goal is to build minimal VM image template which utilizes 'cloud-init' to declare initial VM configuration during the first bootup. Aim is to avoid VM cloning, scripting and manual exeution of commands to initialize new VM images in QNAP NAS.

## Preparations for 'cloud-init' using its NoCloud datasource in QNAP NAS (requires admin SSH access into NAS)
Create symbolic links for tools to ease operation

    # ln -s /share/CACHEDEV1_DATA/.qpkg/QKVM/usr/bin/qemu-img /usr/sbin/qemu-img
    # ln -s /share/CACHEDEV1_DATA/.qpkg/QKVM/usr/bin/virsh /usr/sbin/virsh <-- not needed ???

Create a 'raw' virtual disk for 'cloud-init' seed input use

    # dd if=/dev/zero of=cloud-init.vdisk bs=512 count=2880

Format virtual disk, create FAT32 filesytem on it and name it as 'cidata'

    # mkfs.vfat -F 16 -n cidata cloud-init.vdisk

Mount virtual disk to transfer 'cloud-init' configuration files into the disk

    # mount -t vfat -o loop cloud-init.vdisk vdisk

Download Ubuntu Minimal Server cloud image from

    https://cloud-images.ubuntu.com/minimal/

Check that image format is qcow2 and rename it as a base image

    # qemu-img info ubuntu-18.04-server-cloudimg-amd64_20200412_192114.img
    # mv ubuntu-18.04-server-cloudimg-amd64_20200412_192114.img ubuntu-server-18.04.qcow2

Create a new VM instance image and resize it (following Virtualization Station naming practice for instance images)

    # mkdir ubuntu-server
    # qemu-img create -f qcow2 -o size=32G -o backing_file=ubuntu-server-18.04.qcow2 ubuntu-server/ubuntu-server_disk1.img
    # qemu-img create -f qcow2 - ubuntu-server-18.04.qcow2 ubuntu-server/ubuntu-server_disk1.img
    # cd ubuntu-server
    # qemu-img resize ubuntu-server_disk1.img 32G
    

Resize the image

    # 


Disable 'cloud-init' after initial boot (check if this is really needed anymore)

    $ sudo touch /etc/cloud/cloud-init.disabled

## References
- https://wiki.ubuntu.com/Minimal  
- https://cloud-images.ubuntu.com/minimal/  
- https://cloud-init.io/  
- https://cloudinit.readthedocs.io/en/latest/  
- https://www.qemu.org/docs/master/tools/index.html  
- https://www.tecmint.com/create-virtual-harddisk-volume-in-linux/  
- https://medium.com/@artem.v.vasilyev/use-ubuntu-cloud-image-with-kvm-1f28c19f82f8  
- https://documentation.suse.com/sles/15-SP1/html/SLES-all/cha-qemu-guest-inst.html#cha-qemu-guest-inst-qemu-img  





Create partition table for virtual disk

    # fdisk test.vdisk
    Command (m for help): x
    
    Expert command (m for help): c
    Number of cylinders (1-1048576): 80
    Expert command (m for help): r

    Command (m for help): n
    Command action
    e   extended
    p   primary partition (1-4)
    -> select 'p'
    Partition number (1-4): 1
    First cylinder (1-80, default 1): 1
    Last cylinder or +size or +sizeM or +sizeK (1-80, default 80): 80

    Command (m for help): t
    Selected partition 1
    Hex code (type L to list codes): b

