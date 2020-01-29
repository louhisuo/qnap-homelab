# Installing Kubernetes with Kubeadm
Step-by-step guide to install three node (1 x Master and 2 x Workers) k8s cluster on Ubuntu 18.04 LTS based VMs.  
It is assumed that you have preparated VMs required for k8s installation and installed a container runtime. All commands below should be executed in all nodes, unless stated otherwise.

Configure prerequisites for k8s and kubeadm in all VMs.
---
Disable swap (required by k8s)

    $ sudo swapoff --all

Check that iptables version

    $ iptables --version

If needed (iptables version is > 1.8) configure iptables to use its legacy backend, not nftables (required by kubeadm / kube-proxy)
    
    $ sudo apt install iptables arptables ebtables
    $ sudo update-alternatives --set iptables /usr/sbin/iptables-legacy
    $ sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy
    $ sudo update-alternatives --set arptables /usr/sbin/arptables-legacy
    $ sudo update-alternatives --set ebtables /usr/sbin/ebtables-legacy

Install k8s packages
---
Setup the official kubernetes package repository

    $ curl -s https://apt.kubernetes.io/doc/apt-key.gpg | sudo apt-key add -
    $ sudo add-apt-repository "deb https://apt.kubernetes.io/ kubernetes-xenial main"

Install kubeadm, kubectl and kubelet

    $ sudo apt install kubeadm kubectl kubelet
    $ sudo apt-mark hold kubeadm kubectl kubelet

Restart kubelet service

    $ sudo systemctl daemon-reload
    $ sudo systemctl restart kubelet

Initialize k8s control plane

    $ sudo kubeadm init

---
References:  
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/  
https://k8s-school.fr/resources/en/blog/kubeadm/  
https://www.codeproject.com/Articles/5065989/3-Nodes-Kubernetes-Cluster-for-Home-Lab-Quick-Dirt  
