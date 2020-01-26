# Installing Docker CE on Ubuntu 18.04 LTS server
A short instruction how I make a clean installation of Docker CE and Docker Compose for my docker and kubernetes homelabs. See references for inspiration and sources of knowledge which I have used to create this instruction.  

(1) Remove old docker versions including their configuration files, in case any previous docker packages exist.  
    $ sudo apt purge docker docker-engine docker.io containerd runc docker-compose  

    Remove any previous images, containers, volumes, or customized configuration files (optional step).  
    Have not tested myself, but 'purge' may wipe content of /var/lib/docker. So if unsure, use 'remove' in above step.  
    This step is to make sure nothing old is left behind.  
    $ sudo rm -rf /var/lib/docker

(2) Setup the official Docker CE repository (stable) for package management  
    $ sudo apt update  
    $ sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common  
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -  
    $ sudo apt-key fingerprint 0EBFCD88  
    $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"  

(3) Install the latest office Docker CE package  
    $ sudo apt install docker-ce docker-ce-cli containerd.io  
    
(4) Install the latest Docker Compose package (optional step)
    $ sudo apt install docker-compose  
    
(5) Simple validation of your Docker installation
    $ sudo docker version
    $ sudo docker info
    $ sudo docker-compose version
    $ sudo docker run hello-world

---
References:  
https://fabianlee.org/2019/09/28/docker-installing-docker-ce-on-ubuntu-bionic-18-04/  
https://docs.docker.com/install/linux/docker-ce/ubuntu/  
http://manpages.ubuntu.com/  
