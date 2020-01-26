# Installing Docker CE on Ubuntu 18.04 LTS server
A short instruction how I make clean install of Docker CE for my docker and kubernetes homelabs. See references for sources I have used to create this instruction.  

(1) Remove old docker versions including their configuration files, in case any previous docker packages exist  
$ sudo apt purge docker docker-engine docker.io containerd runc  



---
References:  
https://docs.docker.com/install/linux/docker-ce/ubuntu/  
https://fabianlee.org/2019/09/28/docker-installing-docker-ce-on-ubuntu-bionic-18-04/  
