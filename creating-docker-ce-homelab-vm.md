# Installing Docker CE on Ubuntu 18.04 LTS server
A short instruction how I make a clean installation of Docker CE and Docker Compose for my docker homelabs. See references section for inspiration and sources of know-how that have been used to create this guide.  

Prerequisites
---
A cloned VM has been previously created using 'cloning-vm.md' guide.

Pre-installation stpes
---
Remove any old docker versions including their configuration files. Also remove any previous images, containers, volumes, or customized configuration files. So if unsure, do not remove '/var/lib/docker' and use 'remove' apt option instead of 'purge'.

    $ sudo apt purge docker docker-engine docker.io containerd runc docker-compose  
    $ sudo rm -rf /var/lib/docker

Setup the official Docker CE repository (stable) for package management  

    $ sudo apt update  
    $ sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common  
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -  
    $ sudo apt-key fingerprint 0EBFCD88  
    $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"  

Installing Docker CE
---
Install the latest office Docker CE package  

    $ sudo apt install docker-ce docker-ce-cli containerd.io  
    
Validate Docker CE installation

    $ systemctl is-enabled docker.service  
    $ sudo docker version
    $ sudo docker info
    $ sudo docker run hello-world

Post installation steps
---
Manage Docker as a non-root user. Create a Docker group and add a user into the group

    $ sudo groupadd docker
    $ sudo usermod -aG docker $USER
    $ newgrp docker

Finetune install to make it possible to use 'docker login' without X11/Gnome by providing pass service for a package 'golang-docker-credential-helpers'. This is needed perhaps due to a missing dependency? Note that a password will be stored as clear text (to be fixed someday ...). 

    $ sudo apt install pass

Optional steps
---
Install the latest Docker Compose (the current latest version is 1.25.4) directly from the official Docker repository. Note that command completion reguires you to start a new bash session to take effect.

    $ sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" \
      -o /usr/local/bin/docker-compose
    $ sudo chmod +x /usr/local/bin/docker-compose
    $ sudo curl -L https://raw.githubusercontent.com/docker/compose/1.25.4/contrib/completion/bash/docker-compose -o \         
      /etc/bash_completion.d/docker-compose
    $ docker-compose version


References: 
---
https://fabianlee.org/2019/09/28/docker-installing-docker-ce-on-ubuntu-bionic-18-04/  
https://docs.docker.com/install/linux/docker-ce/ubuntu/  
https://docs.docker.com/install/linux/linux-postinstall/  
https://www.tecmint.com/install-docker-and-run-docker-containers-in-ubuntu/  
https://docs.docker.com/compose/install/


TO-DO:
---
Add resource constrains limits (currently having warning 'No swap limit support'. For details see https://docs.docker.com/config/containers/resource_constraints/
