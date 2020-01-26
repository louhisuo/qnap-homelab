# Installing Docker CE on Ubuntu 18.04 LTS server
A short instruction how I make a clean installation of Docker CE and Docker Compose for my docker and kubernetes homelabs. See references section for inspiration and sources of know-how that have used to create this simple guide.  

Remove any old docker versions including their configuration files. Also remove any previous images, containers, volumes, or customized configuration files. So if unsure, do not remove '/var/lib/docker' and use 'remove' apt option instead of 'purge'.

    $ sudo apt purge docker docker-engine docker.io containerd runc docker-compose  
    $ sudo rm -rf /var/lib/docker

Setup the official Docker CE repository (stable) for package management  

    $ sudo apt update  
    $ sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common  
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -  
    $ sudo apt-key fingerprint 0EBFCD88  
    $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"  

Install the latest office Docker CE package  

    $ sudo apt install docker-ce docker-ce-cli containerd.io  
    
Validate Docker CE installation

    $ sudo systemctl is-enabled docker.service  
    $ sudo docker version
    $ sudo docker info
    $ sudo docker run hello-world

Optional step: Install the latest Docker Compose package

    $ sudo apt install docker-compose
    $ sudo docker-compose version
    
---
References:  
https://fabianlee.org/2019/09/28/docker-installing-docker-ce-on-ubuntu-bionic-18-04/  
https://docs.docker.com/install/linux/docker-ce/ubuntu/  
http://manpages.ubuntu.com/  
