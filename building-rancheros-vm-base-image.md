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


Create and start VM to boot RancherOS from ISO image. 

Open VM console and confirm that RancherOS is up and running by checking your IP and docker version.   

    $ ip address
    $ docker version

YOU HAVE NOW WORKING SINGLE NODE DOCKER PLAYGROUND BASED ON RANCHEROS READY (things are non-persistant though as ISO was booted to RAM).

Below are instructions to convert non-persistant "playground" as a persistant 'RancherOS base image'.
---
Set password for 'rancher' user using console connection.

    $ sudo password rancher
    
Generate SSH keypair for a remote SSH access from your client machine into RancherOS VM. In your client machine execute following steps.

    client-machine$ ssh-keygen
    Mote: It is highly recommended that you set a passphrase for your keys.

Create a 'cloud-config.yaml' for RancherOS base image VM (example below) in your client machine.

    hostname: rancheros-base
    ssh_authorized_keys:
      - <copy here your 'id_rsa.pub'content you generated in previous step>
    rancher:
      network:
        interfaces:
          eth0:
            dhcp: true
          # overwrite DHCP assigned nameservers
        dns:
          nameservers:
            - 1.1.1.1
            - 1.0.0.1
          override: true

Copy 'cloud-init.yaml' into RancherOS virtual machine. At client machine execute following

    client-machine$ sftp rancher@<rancheros vm ip address>
    > put clouf-init.yaml
    > exit

Install RancherOS to a disk to make it persistant. Execute following in RancherOS console.

    $ sudo ros install -c cloud-config.yaml -d /dev/sda
    Note, answer 'y' to confirm installation.
 
 When you get a promt "Continue with reboot [y/N]: execute following steps in Virtualization station
 
    - Shutdoewn VM
    - Remove mount to 'rancheros.iso'from VM (click 'Eject CD image at CD/DVD configuration section)
    - Start VM

Confirm that a persistant RancherOS base image VM installation was successful.

    In client machine execute following to login into VM using SSH keys
    client-machine$ ssh -l rancher <rancheros vm ip>
    Inside RancherOS VM execute following command to confirm that docker is running
    $ docker version

Post installation steps
---
Enable VM helper tools (qemu-guest-agent) on VM

    $ sudo ros service list
    $ sudo ros service enable qemu-guest-agent
    $ sudo ros service up qemu-guest-agent


References:
- https://rancher.com/rancher-os/  
- https://rancher.com/docs/os/v1.x/en/overview/  
- https://github.com/rancher/os/blob/master/README.md  
- https://linuxhint.com/install_rancher_os/  
- https://distrowatch.com/table.php?distribution=rancheros  
- https://vitobotta.com/2019/06/25/setting-up-rancheros-for-rancher-and-kubernetes/  
