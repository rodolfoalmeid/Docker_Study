Kubernetes CKA Study
=======================

> Rodolfo R. de Almeida | November 11th, 2021

------------------------------------------

This repository will be used to post all topics related to Kubernetes CKA certification. My study will be based on the CLoud Guru training called "Certified Kubernetes Administrator (CKA)".

*[I am using this basic writing format to create this document.](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)*

------------------------------------------

# Table of Contents

1. [CKA Certification Exam](#CKA-Certification-Exam)
2. [K8S Getting Started](#k8s-Getting-Started)
3. [LAB #01](#LAB-01)

---------------

CKA Certification Exam
====================

## CKA Exam
The CKA exam is periodically updated. The current CKA version is 1.24 and the most significant change is that the exam now uses K8s version 1.24.
The exam version maches the kubernetes version.

## PSI Bridge Proctoring Platform
The exam platform is PSI Bridge Proctoring Platform and it uses the PSI Secure Browser that is required to do the exam.
It is permited to use Documentation link but no personal bookmarks.
It is permited only one active monitor during the exam.


K8s Getting Started
====================

## K8s Basics Quick Recap

* What is kubernetes?
 
  Kubernetes is a portable, extensible, open-source platform for manageing containerized workloads and services, that facilitates both declarative configuration and automation.

* What does "k8s" mean?

  K8s is simply a short for Kubernetes. The 8 represents the 8 letters between K ad S!
 
* What K8s actualy does?

  Kubernetes creates a cluster to manage all nodes (servers) and conteiners is a centralized way. When you run a conteiner inside a Kubernetes cluster you do not need to worry in which server the conteiner will run, because kubernetes will manage that.
 
* K8s Features

  Container Orchestration - The primary purpose of Kubernetes is to dynamicaly manage conteiners across multipe host systems.
  
  Application Reliability - Kubernetes makes it easier to build reliable, self-healing, and scalable applications.
  
  Automation - Kubernetes offers a variety of features to help automate the management of your container apps.

  
## K8s Architectural Overview

* K8s Control Plane

  It is what runs our kubernetes cluster.
  A collection of multiple components that are responsible to manage the cluster globally.
  Control plane components can run on any machine in the cluster, but usually are run on dedicated controller machines.
  
  * **kube-api-server** serves the Kubernetes API, the primary interface to the control plane and the cluster itself.
    When interacting with your Kubernetes cluster, you will usually do so using the Kubernetes API.
    
    *[If you do not know what is API please check this link.](/whatisAPI.md)*
   
    
  * **Etcd** is the backend data store for the kubernetes cluster, and provides high availability sotrage for all data relating to the state of the cluster.
   
  * **Kube-scheduler** handles scheduling, the process of selecting an available node in the cluster on which to run containers.
  
  * **kube-controller-mnager** runs a collection of multiple controller utilities in a single process. These controllers carry out a variety of automation-related tasks within the Kubernetes cluster.
   
  * **cloud-controller-manager** provides an interface between Kubernetes and various cloud platorms. It is only used when using cloud-based resources alongside Kubernetes.
  
* K8s Nodes
  
  Nodes are the machines where the conteiners managed by the cluster run. A cluster can have any number of nodes. Various node components manage conteiners on the machine and communicate with the control plane.
  
  * **Kubelet** is the Kubernetes agent that runs on each node. It communicates with the control plane and ensures that containers are run on its node as instructed by the control plane. It also handles the process of reporting container status and other data about containers back to the control plane.
  
  * **Container Runtime** is not built into Kubernetes. It is a separate piece of software that is responsible for actually running containers on the machine. Kubernetes supports multiple container runtime implementations. Some popular container runtimes are Docker and Containerd.
    
  * **kube-proxy** is a network proxy. It runs on each node and handles some tasks related to providing networking between containers and services in the cluster.

* A K8s Cluster

  This is what the hole kubernetes cluster lookslike.
  
![image](https://user-images.githubusercontent.com/113181949/201423697-3e8189af-fbf5-438d-93c8-f1fa11e643bd.png)

     
## Building a Kubernetes Cluster

### What is kubeadm?

It is a tool that will simplify the process of setting up our Kubernetes cluster. This tool provide `kubeadm init` and `kubeadm join` as best-practice "fast paths" for creating Kubernetes clusters. kubeadm performs the actions necessary to get a minimum viable cluster up and running. By design, it cares only about bootstrapping, not about provisioning machines. Likewise, installing various nice-to-have addons, like the Kubernetes Dashboard, monitoring solutions, and cloud-specific addons, is not in scope.


### Playground Server Setup

![image](https://user-images.githubusercontent.com/113181949/207026751-965e7f24-2f39-4123-8d01-07bd0aeca4d6.png)

## LAB-01

1. Deploy three nodes in your cloud provider.

2. Change the server host name to controller and worker nodes.

   `sudo hostnamectl set-hostname k8s-controller1`

3. Setup the host file with all servers hostname for name resolution.
   
   `sudo vi /etc/hosts`
   
   Exemple to add the IP Address and hostname server. 
   
   *Allways uses private IP Address because they are static. Public IP Address is dynamic and might change*
   
   ![image](https://user-images.githubusercontent.com/113181949/207029232-8bab9ee2-827b-4087-b79a-dd39c3a9045b.png)
   
   > __Note__ 
   > In order to hostname changes take effect, logout and login back to the server.

4. Install and configure conteinerd
   
   - Enable `overlay` and `br_netfilter`. Load kernel modules on startup and everytime the server boots `overlay` and `br_netfilter` will be enabled.
   
   ```
   cat << EOF | sudo tee /etc/modules-load.d/conteinerd.conf 
   overlay
   br-netfilter
   EOF
   ```
   
   - This commands will immediatly enable those modules and it is not required to reboot the host to enable them.
   
   ```
   sudo modprobe overlay
   sudo modprobe br_netfilter
   ```
   
   - Network configurations required that kubernetes is gonna need.
   
   ```
   cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
   net.bridge.bridge-nf-call-iptables = 1
   net.ipv4.ip_forward = 1
   net.bridge.bridge-nf-call-ip6tables = 1
   EOF
   ```
   
   - To apply all this configurations 

   ```
   sudo sysctl --system
   ```
   
5. Install and configure containerd.

   ```
   sudo apt-get update && sudo apt-get install -y containerd
   sudo mkdir -p /etc/containerd
   sudo containerd config default | sudo tee /etc/containerd/config.toml
   sudo systemctl restart containerd
   ```

6. On all nodes, disable swap.
  
   What is Swap on linux? 
   
   Swap space in Linux is a part of a machine's virtual memory used when the physical memory runs out of space. Especially when working with a large program or application where a need for extra memory arises, swap space is an ideal choice to hold temporarily inactive memory pages.
   Sometimes you need to disable Swap on a Linux server. Kubernetes worker nodes need to have Swap turned off in order to increase the performance and stability.
   
   ```
   sudo swapoff -a
   ```

7. On all nodes, install kubeadm, kubelet, and kubectl.

   ```
   sudo apt-get update && sudo apt-get install -y apt-transport-https curl
   
   curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
   
   cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
   deb https://apt.kubernetes.io/ kubernetes-xenial main
   EOF
  
   sudo apt-get update
  
   sudo apt-get install -y kubelet=1.24.0-00 kubeadm=1.24.0-00 kubectl=1.24.0-00
  
   sudo apt-mark hold kubelet kubeadm kubectl
   ```

8. On the control plane node only, initialize the cluster and set up kubectl access.
  
   ```
   sudo kubeadm init --pod-network-cidr 192.168.0.0/16 --kubernetes-version 1.24.0
   
   mkdir -p $HOME/.kube
   
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```
  
9. Verify the cluster is working.

    ```
    kubectl get nodes
    ```
   
10. Install the Calico network add-on.

    ```
    kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
    ```
   
11. Get the join command (this command is also printed during kubeadm init . Feel free to simply copy it from there).
    
    ```
    kubeadm token create --print-join-command
    ```
   
12. Copy the join command from the control plane node. Run it on each worker node as root (i.e. with sudo ).

    ```
    sudo kubeadm join ...
    ```
   
13. On the control plane node, verify all nodes in your cluster are ready. Note that it may take a few moments for all of the nodes to

    ```
    enter the READY state.
    kubectl get nodes
    ```
