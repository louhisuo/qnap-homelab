# Installing Kubernetes with Kubeadm
Step-by-step guide to install three node (1 x Master and 2 x Workers) k8s cluster on Ubuntu 18.04 LTS based VMs.

Configure prerequisites for k8s and kubeadm in all VMs.

Disable swap

  $ sudo swapoff --all
  

---
References:
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/  
https://k8s-school.fr/resources/en/blog/kubeadm/  
https://www.codeproject.com/Articles/5065989/3-Nodes-Kubernetes-Cluster-for-Home-Lab-Quick-Dirt  
