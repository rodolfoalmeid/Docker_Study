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
3. [Building a Kubernetes Cluster](#building-a-kubernetes-cluster)
4. [LAB01 Building a Kubernetes Cluster](#lab01-building-a-kubernetes-cluster)
5. [Namespaces in K8s](#namespaces-in-k8s)
6. [LAB02 Working with Kubernetes Namespaces](#LAB02-working-with-kubernetes-namespaces)
7. [Kubernetes Management](#kubernetes-management)
8. [LAB03 Performing Kubernetes Upgrade](#LAB03-performing-kubernetes-upgrade)
9. [Backing Up and Restoring Etcd Cluster Data](#backing-up-and-restoring-etcd-cluster-data)
10. [LAB04 Backing Up and Restoring Etcd](#LAB04-backing-up-and-restoring-etcd)
11. [Working with kubectl](#working-with-kubectl)
12. [kubectl Tips](#kubectl-tips)
13. [RBAC in k8s](#rbac-in-k8s)
14. [Creating Service Accounts](#creating-service-accounts)
15. [Inspecting Pod Resource Usage](#inspecting-pod-resource-usage)
16. [Managing Application Configuration](#managing-application-configuration)
17. [Managing Containers Resources](#managing-containers-resources)
18. [Monitoring Container Health with Probes](#monitoring-container-health-with-probes)
19. [Building Self-Healing Pods](#building-self-healing-pods)
20. [Multi-Container Pod](#multi-container-pod)
21. [Introducing Init Containers](#introducing-init-containers)
22. [Exploring k8s Scheduling](#exploring-k8s-scheduling)
23. [Using DaemonSets](#using-daemonsets)
24. [Using Static Pods](#using-static-pods)
25. [k8s Deployment Overview](#k8s-deployment-overview)
26. [Scaling Applications with Deployments](#scaling-applications-with-deployments)
27. [Managing Rolling Updates with Deployments](#managing-rolling-updates-with-deployments)
28. [k8s Networking Architectural Overview](#k8s-networking-architectural-overview)
29. [CNI Plugins Overview](#cni-plugins-overview)
30. [Understanding k8s DNS](#understanding-k8s-dns)
31. [Using NetworkPolicies](#using-networkpolicies)
32. [k8s Service Overview](#k8s-service-overview)
33. [Using k8s Services](#using-k8s-services)
34. [Discovering k8s Services with DNS](#discovering-k8s-services-with-dns)
35. [Managing Access from Outside with k8s Ingress](#managing-access-from-outside-with-k8s-ingress)
36. [k8s Storage Overview](#k8s-storage-overview)
37. [Using k8s Volumes](#using-k8s-volumes)
38. [Exploring k8s Persistent Volumes](#exploring-k8s-persistent-volumes)

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

     
Building a Kubernetes Cluster
====================

### What is kubeadm?

It is a tool that will simplify the process of setting up our Kubernetes cluster. This tool provide `kubeadm init` and `kubeadm join` as best-practice "fast paths" for creating Kubernetes clusters. kubeadm performs the actions necessary to get a minimum viable cluster up and running. By design, it cares only about bootstrapping, not about provisioning machines. Likewise, installing various nice-to-have addons, like the Kubernetes Dashboard, monitoring solutions, and cloud-specific addons, is not in scope.


### Playground Server Setup

![image](https://user-images.githubusercontent.com/113181949/207026751-965e7f24-2f39-4123-8d01-07bd0aeca4d6.png)

LAB01 Building a Kubernetes cluster
====================

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
Namespaces in K8S
====================

Namespaces are virtual clusters backed by the same physical cluster. Kubernetes objects, such as pods and containers, live in namespaces. Namespaces are a way to separate and organize objects in your cluster.

## List existing Namespaces

You can list existing namespaces with kubectl:

```
kubectl get namespaces
```

All clusters have a default namespace. This is used when no namespace is specified. kubeadm also creates a kube-system namespace for system components.

## Specify a Namespace

When using kubectl, you may need to specify a namspace. You can do this with the `--namespace` flag.

Alternatively you can use the -n flag, which is the short form of the --namespace flag.

```
kubectl get pods --namespaces my-namespace
```

You can also get pods from all namespaces using the command below.

```
kubectl get pods --all-namespaces
```

> __Note__ 
> If you do not specify a namespace, the default namespace is assumed.


## Create a Namespace

You can create a namespace with kubectl.

```
kubectl create namespace my-namespace
```

LAB02 Working with Kubernetes Namespaces
========================================

## Introduction
Namespaces are a central component of any Kubernetes infrastructure. This lab will give you the opportunity to work with namespaces in a functioning cluster. You will be able to practice the process of creating, using, and navigating Kubernetes namespaces.

## Solution
Log in to the provided control plane node server using the credentials provided:
```
ssh cloud_user@<PUBLIC_IP_ADDRESS>
```

Create the dev Namespace
Create a namespace in the cluster called dev:
```
kubectl create namespace dev
```

Get a List of the Current Namespaces
List the current namespaces:
```
kubectl get namespace
```

Save the namespaces list to a file:
```
kubectl get namespace > /home/cloud_user/namespaces.txt
```

Verify the list saved to the file:
```
cat namespaces.txt
```

We should see the list of namespaces.

## Find the quark Pod's Namespace

Locate the quark pod:
```
kubectl get pods --all-namespaces
```

Copy the name of the namespace where the quark pod is located.

Create a file in which to save its name: :
```
vi /home/cloud_user/quark-namespace.txt
```

Paste in the name of the `quark` pod's namespace.

Save and exit the file by pressing Escape followed by `:wq.`

Kubernetes Management
====================

## Intruduction to High Availability in K8s

K8s favilitates high-availability applications, but you can also design the cluster itself to be high available. To do this, you need multiple control plane nodes.

![image](https://user-images.githubusercontent.com/113181949/209573691-b12e1c73-0391-400e-a931-e90eb36c2ce7.png)

## Stacked etcd
The etcd runs on the same nodes as the rest of control plane components.
This is the setup pattern used by clusters that are set up using kubeadm. So we used kubeadm to build our cluster and kubeadm uses this stacked etcd methodology. That means that our control plane node, which is running all of the different control plane components, is also running etcd, and in a high availability setup, each one of those individual control plane nodes would have its own etcd instance.

![image](https://user-images.githubusercontent.com/113181949/209573214-7246b411-2c80-4353-be8b-7e8e6209085f.png)

## External etcd

With external etcd, we have etcd running on completely separate nodes. In the context of high availability, we could then have multiple external etcd nodes in a high availability cluster, and that set of servers would be a completely different set of servers from the
ones that are running our normal Kubernetes control plane components. With the external etcd model, you can have any number of Kubernetes control plane instances and any number of SED nodes. So in this lesson, we've provided a brief high-level overview of what high availability looks
like in a Kubernetes cluster.


![image](https://user-images.githubusercontent.com/113181949/209573266-fc3a280c-fa21-4fee-ac19-f460ff6ea63e.png)


 > __Note__ 
 > Link to access [Options for High Availability Topolology](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/ha-topology/)


## Introduction to K8s Management Tools

What are k8s Managemente tools?

There is a variety of management tools available for kubernetes. THese tools interface with kubernetes to provide additional functionality. When using Kubernetes, it is a good idea to be aware of some of these tools.

- kubectl
  
  kubectl is the official command line interface for Kubernetes.

- kubeadm
  
  It is a tool for quickly and easily creating Kubernetes clusters.
  
- Minikube
  
  Allows you to automatically set up a local single-node Kubernetes cluster. It igreat for getting k8s up adn running quickly for test and development purposes.
  
- Helm
 
  Provides templating and package management for Kubernetes objects. Can be used to manage your own templates (known charts). You can also download and use shared templates.
  
- Kompose
  
  It helps you translate Docker compose files into Kubernetes objects. If you are using Docker compose for some part of your workflow, you can move your application to Kubernetes easily with Kompose.
  
- Kustomize
-
  It is a configuration management tool for managing Kubernetes object configurations. It allows you to share and re-use templated configurations for Kubernetes applications. It is similar to Helm.
  
  
## Safely Draining a K8s Node

#### What is Draining?

When performing maintenance, you may sometimes need to remove a Kubernetes node from service. To do this, you can drain the node. Containers running on the node will be gracefully terminated (and potentially rescheduled on another node).

#### Draining a Node

To drain a node, use the kubectl drain command.
```
kubectl drain <node name>
```

#### Ignoring DaemonSets

  When draining a node, you may need to ignore DaemonSets (pods that are tied to each node). If you have any DaemonSet pods running on the node, you will likely need to use the ```--ignore-daemonsets``` flag.
  
 ```
 kubectl drain <node name> --ignore-daemonsets
 ```
 
#### Uncordoning a Node

 If the node reamins part of the cluster, yo can allow pods to run on the node again when maintenance is complete using the ```kubectl uncordon``` command.

```
kubectl uncordon <node name>
```

  > __Note__  
   > Be aware that pods running on the Drained node will be started on the available nodes of the cluster, but once the node is uncordoned, it will not be started back in the uncordoned node. The pods will remain running on the nodes and just new pods will be started on the uncordoned node.

  > __Note__  
   > Pods will be started in other nodes if they were deployed as deployments. If a pod is deployed manually in a node, it cannot be started in a different node. The node will be deleted if using ```--force``` flag.


## Upgrading K8s with kubeadm

#### Upgrading with kubeadm
When using kubernetes, you will likely want to periodically upgrade Kubernetes to keep your cluster up to date and kubeadm makes this process easier.

#### Control Plane Upgrade Steps
- Upgrade kubeadm on the control plane node.
- Drain the control plane node.
- Plan the upgrade (kubeadm upgrade plan).
- Apply the upgrade (kubeadm upgrade apply).
- Upgrade kubelet and kubectl on the control plane node.
- Uncordon the control plane node.

#### Worker Node Upgrade Steps
- Drain the node.
- Upgrade kubeadm.
- Upgrade the kubelet configuration (kubeadm upgrade node).
- Upgrade kubelet and kubectl.
- Uncordon the node.

#### Demonstration Upgrading K8s with kubeadm

Drain the control plane node.
```
kubectl drain k8s-control --ignore-daemonsets
```

Upgrade kubeadm.
```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubeadm=1.22.2-00
```
Check the version
```
kubeadm version
```

Plan the upgrade.
```
sudo kubeadm upgrade plan v1.22.2
```

Upgrade the control plane components.
```
sudo kubeadm upgrade apply v1.22.2
```

Upgrade kubelet and kubectl on the control plane node.
```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubelet=1.22.2-00 kubectl=1.22.2-00
```

Restart kubelet.
```
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

Uncordon the control plane node.
```
kubectl uncordon k8s-control
```

Verify that the control plane is working.
```
kubectl get nodes
```

Upgrade the worker nodes.

> __Note__ : In a real-world scenario, you should not perform upgrades on all worker nodes at the same time. Make sure
enough nodes are available at any given time to provide uninterrupted service.

Run the following on the control plane node to drain worker node 1:
```
kubectl drain k8s-worker1 --ignore-daemonsets --force
```

Log in to the first worker node, then Upgrade kubeadm.
```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubeadm=1.22.2-00
```

Upgrade the kubelet configuration on the worker node.
```
sudo kubeadm upgrade node
```

Upgrade kubelet and kubectl on the worker node.
```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubelet=1.22.2-00 kubectl=1.22.2-00
```

Restart kubelet.
```
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

From the control plane node, uncordon worker node 1.
```
kubectl uncordon k8s-worker1
```

Repeat the upgrade process to the other worker nodes.

Verify that the cluster is upgraded and working.
kubectl get nodes


LAB03 Performing Kubernetes Upgrade
===============

Kubernetes Cluster

![image](https://user-images.githubusercontent.com/113181949/217599835-b9b34622-2cbe-4f06-83aa-60339c073d51.png)

In this cluster we have three servers; One controle plane and two worker nodes. The cluster was built with Kubeadm.
First we need to upgrade the control plane and then the worker nodes.


Backing Up and Restoring Etcd Cluster Data
====

### Why Back UP etcd?

**etcd** is the backend data data storage solution for your kubernetes cluster. As such, all your kubernetes objects, applications, and configurations are stored in etcd.

Therefore, you will likely data by backing up etcd.


### Backing UP etcd

You can back up etcd command line tool, **etcdctl**.

Use the **etcdctl snapshot save** command to back up the data. 

![image](https://user-images.githubusercontent.com/113181949/217636700-dc24e7bc-17e0-4dba-bb5f-f826dca6e458.png)


### Restoring etcd

You can restore etcd data from a backup using the **etcdctl snapshot restore** command.

You will need to supply some additional parameters, as the restore operation creates a new logical cluster.

![image](https://user-images.githubusercontent.com/113181949/217636989-6ddff387-e3a5-449e-9789-55e7ed755b82.png)


LAB04 Backing Up and Restoring Etcd
===

![image](https://user-images.githubusercontent.com/113181949/218560253-218ae9af-b973-4417-a331-31ac0bced929.png)

#### Step 01 - Check the variable

```
ETCDCTL_API=3 etcdctl get cluster.name \
--endpoints=https://10.0.1.101:2379 \
--cacert=/home/cloud_user/etcd-certs/etcd-ca.pem \
--cert=/home/cloud_user/etcd-certs/etcd-server.crt \
--key=/home/cloud_user/etcd-certs/etcd-server.key
```

The output should be something similar to:

![image](https://user-images.githubusercontent.com/113181949/218561066-5ed91258-9b4d-40b8-9ac7-79166822b4be.png)


#### Step 02 - Taking a Snapshot from Etcd data base

```
ETCDCTL_API=3 etcdctl snapshot save /home/cloud_user/etcd_backup.db \
--endpoints=https://10.0.1.101:2379 \
--cacert=/home/cloud_user/etcd-certs/etcd-ca.pem \
--cert=/home/cloud_user/etcd-certs/etcd-server.crt \
--key=/home/cloud_user/etcd-certs/etcd-server.key
```

The output should be something similar to:

![image](https://user-images.githubusercontent.com/113181949/218561856-c6ad1626-e12c-466c-8a0e-eedfdcd3c16b.png)


### Step 03 - Reset the cluster data

```
sudo systemctl stop etcd
```

```
sudo rm -rf /var/lib/etcd
```
r — recusrsive. all directories and subdirectories will be deleted;
f — force. Force delete all files;


```
sudo ETCDCTL_API=3 etcdctl snapshot restore /home/cloud_user/etcd_backup.db \
--initial-cluster etcd-restore=https://10.0.1.101:2380 \
--initial-advertise-peer-urls https://10.0.1.101:2380 \
--name etcd-restore \
--data-dir /var/lib/etcd
```

The output should be something similar to:

![image](https://user-images.githubusercontent.com/113181949/218563687-edee8404-604d-401e-b60d-e0c8510e3441.png)


### Step 04 - Check Etcd 

![image](https://user-images.githubusercontent.com/113181949/218564201-6d3414fd-1043-4ce6-a3da-66d9f72ddb63.png)


Working with kubectl
===

### What is kubectl?

kubectl is a command line tool that allows you to interact with Kubernetes. kubectl uses the Kubernetes API to communicate with the cluster and carry out your commands. You can use kubectl to deploy applications, inspect and manage cluster resources, and view logs.

### kubectl get

Use **kubectl get** to list objects in the Kubernetes cluster.

- `-o`  - Set the output format
   
   ```kubectl get pods -o wide``` >> Wide provides more detailed information about the command.
   
   ```kubectl get pods -o json``` >> Gives the output in json format
   
   ```kubectl get pods -o yaml``` >> Gives the output in yaml format
   
- `--sort-by`  - Sort output using a JSONPath expression.
   
- `--selector`  - Filter results by label.

   ```kubectl get pods -n kube-system --selector k8s-app=calico-node``` >> this will show the pods that contains this lable.


```
$ kubectl get <object type> <object name> -o <output> --sort-by <JSONPath> --selector <selector>
```

### kubectl describe

You can get detailed information about Kubernetes objects using kubectl describe.

```
kubectl describe <object type> <object name>
```

### kubectl create

Use **kubectl create** to create objects.

Supply a YAML file with -f create an object from a YAML descriptor stored in the file.
If you attempt to create an object that already exists, an error will occur.

```
kubectl create -f <file name>
```

### kubectl apply

kubectl apply is similar to kubectl create. However, if you use kubectl apply on an object that already exists, it will modify the existing object, if possible.

```
$ kubectl apply -f <file name>
```

### kubectl delete

Use **kubectl delete** to delete objects from the cluster.

```
kubectl delete <object type> <object name>
```

### kubectl exec

**kubectl exec** can be used to run commnds inside containers. Keep in mind that, in order for a command to succeed, the necessary software must exist within the container to run it.

```
kubectl exec <pod name> -c <container name> -- <command>
```

   > __Note__ 
   >  `-c` is used only if your pod has multiple containers, with that flag you can specify the container.


kubectl Tips
===

## Imperative Commands
There are the Declarative and the Imperative commands in kubernetes.
The **Declarative** command define objects using data structures such as YAML or JSON.
The **Imperative** command define objects using kubectl commands and flags. 

Example of Imperative command:

```
kubectl create deployment my-deployment --image=nginx
```

*[kubectl cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)*

Run kubectl create to see a list of objects that can be created with imperative commands.

```
kubectl create
```

Create a deployment imperatively.

```
kubectl create deployment my-deployment --image=nginx
```

Do a dry run to get some sample yaml without creating the object.

```
kubectl create deployment my-deployment --image=nginx --dry-run -o yaml
```

   > __Note__ 
   > Dry run is an expression used to indicate a rehearsal of a performance or procedure before the real one. Dry run mode gives you the possibility of issuing a command without side effects for testing an actual command that you intend to run.


Save the yaml to a file.

```
kubectl create deployment my-deployment --image=nginx --dry-run -o yaml > deployment.yml
```


Create the object using the file.

```
kubectl create -f deployment.yml
```

Scale a deployment and record the command.

```
kubectl scale deployment my-deployment replicas=5 --record
kubectl describe deployment my-deployment
```

RBAC in k8s
===

Role-based access control (RBAC) in k8s allows you to control what users are allowed to do and access within your cluster. For example, you can use RBAC to allow developers to read metadata and logs from kubernetes pods but not make changes to them.

## RBAC Objects

**Roles** and **ClusterRoles** are kubernetes objects that define a set of permissions. These permissions determine what users can do in the cluster. A Role defines permissions within a particular namespace, and a ClusterRole defines cluster-wide permissions not specific to a single namespace.


**RoleBinding** and **ClusterRoleBinding** are objects that connect Roles and ClusterRoles to users.

![image](https://user-images.githubusercontent.com/113181949/229828229-9b8ed41c-41ba-4657-ba04-1e1c215291e8.png)

## Lesson Reference

Create a Role spec file.

```
vi role.yml
```
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
namespace: default
name: pod-reader
rules:
- apiGroups: [""]
resources: ["pods", "pods/log"]
verbs: ["get", "watch", "list"]
```

Create the Role.

```
kubectl apply -f role.yml
```

Bind the role to the dev user.

```
vi rolebinding.yml
```
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
name: pod-reader
namespace: default
subjects:
- kind: User
name: dev
apiGroup: rbac.authorization.k8s.io
roleRef:
kind: Role
name: pod-reader
apiGroup: rbac.authorization.k8s.io
Create the RoleBinding.
```
```
kubectl apply -f rolebinding.yml
```

Creating Service Accounts
===

## What is a Service Account
Is an account used by container processes within Pods to authenticate with the k8s API.
If your Pods need to communicate with the k8s API, you can use service accounts to control their access.

## Creating Service Accounts
A service account object can be created with some YAML just like any other k8s object.

## Binding Roles to Service Accounts
You can manage access control for service accounts, just like any other user, using RBAC objects. Bind service accounts with RoleBindings or ClusterRoleBindings to provide access to k8s API functionality.

Create a basic ServiceAccount.

```
vi my-serviceaccount.yml
```
```
apiVersion: v1
kind: ServiceAccount
metadata:
name: my-serviceaccount
```
```
kubectl create -f my-serviceaccount.yml
```

Create a ServiceAccount with an imperative command.

```
kubectl create sa my-serviceaccount2 -n default
```

View your ServiceAccount.

```
kubectl get sa
```

Attach a Role to the ServiceAccount with a RoleBinding.

```
vi sa-pod-reader.yml
```
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
name: sa-pod-reader
namespace: default
subjects:
- kind: ServiceAccount
name: my-serviceaccount
namespace: default
roleRef:
kind: Role
name: pod-reader
apiGroup: rbac.authorization.k8s.io
```
```
kubectl create -f sa-pod-reader.yml
```

Get additional information for the ServiceAccount.

```
kubectl describe sa my-serviceaccount
```

Inspecting Pod Resource Usage
===

## Kubernetes Metrics Server

In order to view metrics about the resources pods and containers are using, we need an add-on to collect and provide that data. One such add-on is **Kubernetes Metrics Server**. This needs to be manually installed after a kubernetes cluster has been deployed.

## kubectl top
With kubectl top, you can view data about resource usage in your pods and nodes. kubectl top also support flags like --sort-by and --selector.

```
kubectl top pod --sort-by <JSONPATH> --selector <selector>
```
```
kubectl top node
```

## Installing Metrics Server and tesing top command

Install Kubernetes Metrics Server.
```
https://raw.githubusercontent.com/linuxacademy/content-cka-resources/master/metrics-server-components.yaml
```

Verify that the metrics server is responsive. Note that it may take a few minutes for the metrics server to become responsive to
requests.
```
kubectl get --raw /apis/metrics.k8s.io/
```

Create a pod to monitor.
```
vi pod.yml
```
```
apiVersion: v1
kind: Pod
metadata:
name: my-pod
labels:
 app: metrics-test
spec:
containers:
- name: busybox
image: radial/busyboxplus:curl
command: ['sh', '-c', 'while true; do sleep 3600; done']
```
```
kubectl apply -f pod.yml
```

Use  kubectl top  to view resource usage by pod.
```
kubectl top pod
```

Sort output with  --sort-by .
```
kubectl top pod --sort-by cpu
```
Filter output by label with  --selector .
```
kubectl top pod --selector app=metrics-test
```

Managing Application Configuration
===

## Application Configuration

When you are running applications in kubernetes, you may want to pass dynamic values to your applications at runtime to controll how they behave. This is known as **application configuration**
We will pass data to our containers that is going to control what those containers do and how they run.

## ConfigMaps
This is one of the primary ways to store configuration data in kubernetes.
You can store configuration data in kubernetes using Config Maps.
ConfigMaps store data in the form of a key-value map. ConfigMap data can be passed to your container applications.

![image](https://user-images.githubusercontent.com/113181949/230373131-cae47a00-4e6c-4b02-bf55-ae4220683e22.png)

IMPORTANT ITENS!!

**data section**
It is just a yaml data. It is a map of value key pairs

**multi-line data**
You can specify a configuration file to be applyied to one of your containers.


## Secrets
This is another way to store data in kubernetes. Secrets are similar to ConfigMaps but are designed to store sensitive data, such as passwords or API keys, more securely. They are created and used similarly to ConfigMaps.

![image](https://user-images.githubusercontent.com/113181949/230374033-fd18bfb8-c971-4fad-b0c2-bcdf4f52bee7.png)


## Environment Variables
You can pass ConfigMaps and Secret data to your containers as environment variables. These variables will be visable to your container process at runtime.

![image](https://user-images.githubusercontent.com/113181949/230376421-a93094a4-0ab4-42e2-95f0-ffa55be243cf.png)


## Configuration Volumes
Configuration data from ConfigMaps and Secrets can also be passed to containers in the form of **mounted volumes**. This will cause the configuration data to appear in files available to the container file system.
Each top-level key in the configuration data will appear as a file containing all keys below that top-lvel key.

![image](https://user-images.githubusercontent.com/113181949/230378303-d943637c-550d-4dcf-b1df-e1f18a1adda3.png)


## Lesson Reference

[Managing Application Configuration.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11170643/1597437360824-devops-wb002.-.S05-L02.Managing.Application.Configuration.pdf)


Managing Containers Resources
===

## Resource Requests
It allow you to define an amount of resources (such CPU or memory) you expect a container to use. The Kubernetes scheduler will use resource requests to avoid scheduling pods on nodes that do not have nough available resources.

   > __Note__ 
   > Containers are allowed to use more (or less) than the requested resources. Resource requests only affect scheduling.

![image](https://user-images.githubusercontent.com/113181949/230402486-23120793-b732-49d4-8af6-3be54b381d46.png)

   > __Note__ 
   > Memory is measured in bytes and CPU is measured in CPU units.


## Resource Limits
Provide a way for you to limit the amount of resources your containers can use. The container runtime is responsible for enforcing these limits, and different container runtimes do this differently.

   > __Note__ 
   > Some runtimes will enforce these limits by terminating container process that attempt to use more than the allowed amount of resources.

![image](https://user-images.githubusercontent.com/113181949/230403521-30483b72-be09-40a5-ad3d-f3d6f119d93e.png)



Monitoring Container Health with Probes
===

## Container Health
k8s provides a number of features that allow you to build robust solutions, such as the ability to automatically restart unhealthy containers. To make the most of these features, k8s needs to be able to accurately determine the status of your applications. This means actively monitoring container health.

## Liveness Probes
Allow you to automatically determine wether or not a container application is in a healthy state. By default, k8s will only consider a container to be down if the container process stops. Liveness probes allow you to customize this detection mechanism and make it more sphisticated.

![image](https://user-images.githubusercontent.com/113181949/230412110-4f02cf82-a046-4fd1-9047-bc0773649c99.png)


## Startup Probes
Are very similar to liveness probes. However, while liveness probes run contantly on a shcedule, startup probes run at container startup and stop running once they succeed. They are used to determine when the application has successfully started up. Startup probes are especially useful for legacy applications that can have long startup times.

![image](https://user-images.githubusercontent.com/113181949/230412952-892cdcb8-e56c-4322-b51a-d10b5e8a58bd.png)


## Readiness Probes
Are used to determine when a container is ready to accept requests. When you have a service backed by multiple container endpoints, user traffic will not be sent to a particular pod until its containers have all passed the readiness checks defined by their readiness probes. Use readiness probes to prevent user traffic from being sent to pods that are still in the process of starting up.

![image](https://user-images.githubusercontent.com/113181949/230413499-deb6f470-2887-4a93-88f5-50b6be1ce383.png)


## Lesson Reference

[Monitoring Container Health with Probes.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11171607/1597437462380-devops-wb002.-.S05-L04.Monitoring.Container.Health.with.Probes.pdf)


Building Self-Healing Pods
===

## Restart Policies

K8s can automatically restart containers when they fail. **Restart Policies**allow you to customize this behavior by defining when you want a pod's containers to be automatically restarted. Restart policies are an important component of self-healing applications, which are automatically repaired when a problem araises. There are three possible values for a pod's restart policy in k8s: Always, OnFailure, and Never.

### Always
Always is the default restart policy in k8s. With this policy, containers will always be restarted if they stop, even if they completed successfully. Use this policy for applications that should always be running.

### OnFailure
The OnFailure restart policy will restart containers only if the container process exits with an error code or the container is determined to be unhealthy by a liveness probe. Use this policy for applications that need to run successfully and then stop.

### Never
The Never restart policy will cause the pod's containers to never be restarted, even if the containerexits or a liveness probe fails. Use this for applications that should run once and never be automatically restarted.

### Lesson Reference

[1597437528004-devops-wb002 - S05-L05 Building Self-Healing Pods with Restart Policies.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11203458/1597437528004-devops-wb002.-.S05-L05.Building.Self-Healing.Pods.with.Restart.Policies.pdf)


Multi-Container Pod
===

A k8s Pod can have one or more containers. A Pod with more than one container is a multi-container pod. In a multi-container pod, the containers share resources such as network and storage. They can interact with one another, working together to provide functionality.
   > __Note__ 
   > Best Practice: Keep containers in separate Pods unless they need to share resources.

### Cross-Container Interaction

Containers sharing the same Pod can interact with one another using shared resources.

#### Example #01 - Network
Containers share the same networking namespace and can communicate with one another on any port, even if that port is not exposed to the cluster.

#### Example #02 - Storage
Containers can use volumes to share data in a Pod.

#### Example Use Case
There is an application that is hard-coded to write log output to a file o disk. To solve this issue we can add a secondary container to the Pod (sometimes called a **sidecar**) that reads the log from a shared volume and prints it to the console so the log output will appear in the container log.

![image](https://user-images.githubusercontent.com/113181949/236002907-57f56f55-d374-4d05-9f68-8131a252e995.png)

[S05-L06 Creating Multi-Container Pods.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11388676/S05-L06.Creating.Multi-Container.Pods.pdf)


Introducing Init Containers
===
Init containers are containers that run once during the startup process of a pod. A pod can have any number of init containers, and they will each run once (in order) to completion.

![image](https://user-images.githubusercontent.com/113181949/236008629-2090f154-c654-435b-9a5f-da75c52e9a0d.png)


### Init Containers

You can use init containers to perfom a variety of startup tasks. They can contain and use software and setup scripts that are not needed by your main containers. They are often useful in keeping your main containers lighter and more secure by offloading startup tasks to a separate container.

### Use Cases for Init Containers

- Cause a pod to wait for another k8s resource to be created befor fiishing startup.
- Perform sensitive startup setps securely outside of app contianers.
- Populate data into a shared volume at startup.
- Communicate with another service at startup.

### About Init Containers
- They have a different type called ``` initContainers ```
- They will run first during the deployment or daemonset initialization.
- will run in the order listed in the YAML file.
- Each init container need to complete (exit) successfully. If failed the pod will not start depending on the startup policy.

```
apiVersion: batch/v1
kind: CronJob
metadata:
  name: content
spec:
  schedule: "*/60 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          initContainers:
            - name: content
              image: alpine
              command:
                - "/bin/sh"
                - "-c"
                - "rm -rf /tmp/* || true"
              volumeMounts:
                - name: tmp
                  mountPath: /tmp/
          containers:
            - name: content
              image: alpine
              command:
                - /scripts/content-download.sh
```


[S05-L06 Introducing Init Containers.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11389654/S05-L06.Introducing.Init.Containers.pdf)

Exploring k8s Scheduling
===

### Scheduling
The process of assigning Pods to Nodes so kubelets can run them. 

#### Scheduler
Control plane component that handles scheduling.

#### Scheduling Process
The kubernetes scheduler selects a suitable Node for each Pod. It takes into account things like:
- Resources requests vs. available node resources
- Various configurations that affect scheduling using node lables.

### Node Selector
You can configure a nodeSelector for your Pods to limit which Node(s) the Pod can be scheduled on. Node selectors use node lables to filter suitable nodes.

![image](https://user-images.githubusercontent.com/113181949/236030133-c924e097-8655-4213-a7a0-2634a622c1ca.png)

It tells scheduler to only schedule that pod to nodes that only has that lable.


### Node Name

You can bypass scheduling and assign a Pod to a specific Node by name using nodeName.

![image](https://user-images.githubusercontent.com/113181949/236030748-bfb42d2a-aef2-41c0-841c-d8e251314fff.png)

[S06-L01 Exploring k8s Scheduling.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11390675/S06-L01.Exploring.k8s.Scheduling.pdf)


Using DaemonSets
===

### What is DaemonSet?
It automatically runs a copy of a Pod on each node. DaemonSets will run a copy of the Pod on new nodes as they are aded to the cluster.

### DaemonSets and Scheduling
DaemonSets respects normal scheduling rule around node labels, taints, and tolerations. If a pod would not normally be scheduled on a node, a DaemonSet will not create a copy of the Pod on that Node.

[S06-L02 Using DaemonSets.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11398320/S06-L02.Using.DaemonSets.pdf)


Using static Pods
===

### What is a Static Pod
A Pod that is managed directly by the kubelet on a node, not by the k8s API server. They can run even if there is no k8s API server present. Kubelet automatically creates static Pods from YAML manifest files located in the manifest path on the node.

This is the main path to store the YAML manifest files.
```
/etc/kubernetes/manifests/
```
   > __Note__ 
   > If you want to imediatly start the container, just restart the kubelet service  by issuing the command ```systemctl restart kubelet```

### Mirror Pods
Kubelet will create a mirror Pod for each static Pod. Mirror Pods allow you to see the status of the static Pod via k8s API, but you cannot change or manage them via the API.

[S06-L03 Using Static Pods.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11398710/S06-L03.Using.Static.Pods.pdf)


k8s Deployment Overview
===

### What is a Deployment
A k8s object that defines a desired state for a ReplicaSet (a set of replica Pods). The Deployment Controller seeks to maintain the desired state by creating, deleting, and replacing Pods with new configurations.

### Desired State
##### Replicas
The number of replica Pods the Deployment will seek to maintain.

##### Selector
A label selector used to identify the replica Pods managed by the Deployment.

##### Template
A template Pod definition used to create replica Pods.

### Use Cases
There are many use cases for Deployments, such as:
- Easily scale an application up or down by changing the number of replicas.
- Perform rolling updates to deploy a new software version.
- Roll back to a previous software version.

[S07-L01 K8s Deployments Overview.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11400613/S07-L01.K8s.Deployments.Overview.pdf)

Scaling Applications with Deployments
===

### What is scaling?
Scaling rfers to dedicating more (or fewer) resources to an application in order to meet changings needs. k8s Deployments are very useful in horizontal scaling, which involves changing the number of containers running an application.

### Deployment Scaling
The Deployment's replicas setting determines how many replicas are desired in its desired state. If the replicas number is changed, replica Pods will be created or deleted to satisfy the new number.

### How to Scale a Deployment

#### Edit YAML
You can scale a deployment simply by chnaging the number of replicas in the YAML descriptor with ```kubectl apply``` or ```kubectl edit```.

```
spec:
  replicas: 5
```

#### kubectl scale command
Or use the special ```kubectl scale``` command.
```
kubectl scale --replicas=5 deployment.v1.apps/my-deployment 
```

[S07-L02 Scaling Applications With Deployments.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11400697/S07-L02.Scaling.Applications.With.Deployments.pdf)


Managing Rolling Updates with Deployments
===

### What is a Rolling Update?
Rolling updates allow you to make changes to a deployment's Pods at a controlled rate, gradually replacing old Pods with new Pods. This allows you to update your Pods without icurring downtime.

To start a rolling update you just need to edit the Deployment and after saving it, the kubernetes will automatically start the rolling update process and update the pods one by one.

To check the status of the rolling update use the following command:
```
kubectl rollout status deployment.v1.apps/my-deployment
```
Another way to edit a deployment is usign the following command:
```
kubectl set image deployment/my-deployment nginx=nginx:broken --record
```
   > __Note__ 
   > In the above command we set ```:broken``` to define a wrong version.
   > the flag ```--record ``` is used to record the rollout steps.

To check the rollout steps you can use the following command:
```
kubectl rollout history deployment.v1.apps/my-deployment
```


### What is a Rollback?
If an update to a deployment causes a problem, you can roll back the deployment to a previous working state.

To rollback a rolling update you can use the following command:
```
kubectl rollout undo deployment.v1.apps/my-deployment --to-revision=2
```

   > __Note__ 
   > You can use the flag ```--to-revision=2 ``` to rollback to a specific revision.

[S07-L03 Managing Rolling Updates With Deployments.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11400949/S07-L03.Managing.Rolling.Updates.With.Deployments.pdf)

k8s Networking Architectural Overview
===

### k8s Network Model
The k8s network model is a set of standards that define how networking between Pods behaves. There are a variety of different implementations of this model - including the **Calico network plugin**, which we have been using throughout this course.

### Network Model Architecture
The k8s network model defines how Pos communicate with each other, regardless of which Node they are running on. Each Pod has its own **unique IP address** within the cluster. Any Pod can reach any other Pod using that Pod's IP address. This creates a virtual network that allows Pods to easily communicate with each other, regardless of which node they are on.

![image](https://user-images.githubusercontent.com/113181949/236538041-9cd2b3b5-9376-434c-a32b-96a41516cee3.png)


CNI Plugins Overview
===

### CNI Plugins
CNI Plugins are a type of kubernetes network plugin. These plugins provide network conectivity between Pods according to the standard set by the kubernetes network model. 

### Selecting a Network Plugin
Which network plugin is best for you will depend on your specifc situation. Check the kubernetes documentation for  list of available plugins. You may need to research some of these plugins for yourself, depending on your production use case.

### Installing Network Plugins
Each plugin has its own unique installation process.

   > __Note__ 
   > Kubernetes nodes will remain NotReady until a network plugin is installed. You will b unable to run Pods while this is the case.



Understanding k8s DNS
===

### DNS in k8s
The k8s virtual network uses a DNS (Domain Name System) to allow Pods to locate other Pods and Services using domain names instead of IP addresses. This DNS runs as a Service within the cluster. You can usually find it in the **kube-system** namespace.
Kubeadm clusters use **CoreDNS**.


### Pod Domain Names
All Pods in our kubeadm cluster are automatically given a domain name of the following form:
```
pod-ip-address.namespace-name.pod.cluster.local
```

A Pod in the default namespace with the IP Address 192.168.10.100 would have a domain name like this.
```
192-168-10-100.default.pod.cluster.local
```

[S08-L03 Understanding K8s DNS.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11409295/S08-L03.Understanding.K8s.DNS.pdf)


Using NetworkPolicies
===

### What is a NetworkPolicy?
A k8s **NetworkPolicy** is n object that allows you to control the flow of the network communication to and from Pods. This allows you to build a more secure cluster network by keeping Pods isolated from traffic they do not need.

### Pod Selector
**podSelector** determines to which Pods in the namespace the NetworkPolicy applies. The podSelector can select Pods using Pod lables.

![image](https://user-images.githubusercontent.com/113181949/236550142-8854939b-79ba-40e5-ad77-0ad758ca8ce1.png)


   > __Note__ 
   > By default, Pods are considered non-isolated and completely open to all communication.
   > If any NetworkPolicy selects a Pod, the Pod is considered isolated ad will only be open to trafic allowed by NetworkPolicies.


### Ingress and Egress
A NetworkPolicy can apply to Ingress, Egress or both.

![image](https://user-images.githubusercontent.com/113181949/236550251-67afbccf-7388-414d-98b8-4e9acec16305.png)


### from and to Slectors

**from selector**: Selects ingress (incoming) traffic that will be allowed.

**to selector**: Selects egress (outgoing) traffic that will be allowed.

![image](https://user-images.githubusercontent.com/113181949/236550519-a9b1c9b1-a583-43b8-921f-3b082f897e76.png)

![image](https://user-images.githubusercontent.com/113181949/236550630-a45a21df-d1f5-4185-aba0-ff4410d601a6.png)


### Ports
**port**: Specifies one or more ports that will allow traffic. Traffic is only allowed i it matchs both an allowed port and one of the from/to rules.

![image](https://user-images.githubusercontent.com/113181949/236550773-c60dd442-ee92-4e27-9c91-4b3c946e0b8d.png)


[S08-L04 Using NetworkPolicies.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11409474/S08-L04.Using.NetworkPolicies.pdf)


k8s Service Overview
===


### What is a service?
Kubernetes Services provide a way to expose an application running as a set of Pods.
They provide an abstract way for clients to access applications without needing to be aware of the application Pods.


### Service Routing
Clients make requests to a Service, which routes traffic to its pods in a load-balanced fashion.
![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/0af9cfe1-a6c4-4c88-8e1d-1052fa327476)


### Endpoints
Endpoints are the backend entities to which Services route traffic. For a Service that routes traffic to multiple Pods, each Pod will have an endpoint associated to the service.

   > __Note__ 
   > One way to determine which Pod(s) a Service is routing traffic to is to look at the services Endpoints.


Using k8s Services
===

### Service Types
Each Service has a type. The Service type determines how and where the Service will expose your application. There are four service types:

- ClusterIP
- NodePort
- LoadBalancer
- ExternalName (outside the scope of CKA)


### Cluster IP Services
Services expose applications inside the cluster network. Use them when your clients will be other Pods within the cluster.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/9a44ab3d-a1af-4254-9836-b180bfbf3fc2)


### NodePort Services
NodePort Services expose applications outside the cluster network. Use NodePort when applications or users will be accessing yout application from outside the cluster.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/b677b082-dea0-42e4-842b-65e3bf2bedd8)


### LoadBalancer Services
LoadBalancer Services also expose applications outside the cluster network, but they use an external cloud load balancer to do so. This service type only works with cloud platforms that include load balancing functionality.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/f95be9de-5d9a-491a-8650-cc286e6b9dc1)

[S09-L02 Using K8s Services.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11557149/S09-L02.Using.K8s.Services.pdf)


Discovering k8s Services with DNS
===

### Service DNS Names

The Kubernetes DBS (Domain Name System) assigns DNS names to Services, allowing applications within the cluster to easily locate them.

A services fully qualified domain name has the following format:

``` service-name.namespace-name.svc.cluster-domain.example```

The default cluster domain is ```cluster.local```.

### Service DNS and Namespaces

A Services fully qualified domain name can be used to reach the service from within any Namespace in the cluster.

However, Pods withinthe same Namespace can also simply use the service name.

[S09-L03 Discovering K8s Services With DNS.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11558188/S09-L03.Discovering.K8s.Services.With.DNS.pdf)


Managing Access from Outside with k8s Ingress
===

### What is an Ingress?
An Ingress is a kubernetes object that manages external access to Services in the cluster. An Ingress is capable of providing more functionality than a simple NodePort Service, such as SSL termination, advanced load balancing, or name-based virtual hosting.
![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/7c0b331b-c568-45ef-87ee-a2ecdf9b8c0a)


### Ingress Controllers
Ingress objects actually do nothing by themselves. Inorder for Ingresses to do anything, you must install one or more Ingress Controllers. There are a variety of Ingress Controllers available - all of which implement different methds for providing external access to your Services.


### Routing to a Service
Ingress define a set of routing rules. A routing rules properties determine to which requests it applies. Each rule has a set of paths, each with a backend. requests matching a path will be routed to its associated backend. 

In this example a request to ***https://<some-endpoint./somepath*** would be routed to port 80 on the my-service Service.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/97517aa2-1136-43b9-a2c8-bcec6a405978)


### Routing to a Service with NamedPort
If a Service uses a named port, an Ingress can also use the ports name to choose to which port it will route.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/3c816cbb-3790-49a5-b7c4-248612dcff4d)


[S09-L04 Managing Access from Outside with K8s Ingress.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11575857/S09-L04.Managing.Access.from.Outside.with.K8s.Ingress.pdf)


k8s Storage Overview
===

### Container File System
The conainer file system is ephemeral. Files on the containers file system exist only as long as the container exists.
If a container is delete or re-created in k8s, data stored on the container file system is lost.

### Volumes
Many applications need a more persistent method of data storage.
**Volumes** allow you to store data outside the container file system while allowing the container to access the data at runtime.
This can allow data to persist beyond the life of the container.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/5bc2235f-6671-421b-b271-757a0e3ecf9b)



### Persistent Volumes
Volumer offer a simple way to provide external storage to containers within the Pod/container spec.
**Persistent volumes** are slightly more advanced form of Volume. They allow you to treat storage as an abstract resource and consume it using your Pods.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/a3086ec4-acfd-4920-9ff9-7f46f685566b)


### Volume Types
Both Volumes and Persistent Volumes each have a **volume type**. The volume type determines how the storage is actually handled.

Various volume types support storge methods such as:

- NFS
- Cloud storage mechanisms (AWS, Azure, GCP)
- ConfigMaps and Secrets
- A simple directory on the k8s node


Using k8s Volumes
===

### Volumes and volumeMounts
Regular volumes can be set up relatively easily within a Pod/container specification.

**volumes:** In the Pod spec, these specify the storage volumes available to the Pod. They specify the volume type and other data that determines where and how the data is actually stored. You can have any number of volumes in your pod.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/4b983c42-b442-48f8-8980-14039a64b415)

**volumeMounts:** In the container spec, these reference the volumes in the Pod spec and provide a mountPath (the locations on the file system where the container process will access the volume data).

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/7d51980f-76fa-443f-90eb-b12ee2d06870)


### Sharing Volumes between Containers
You can use volumeMounts to mount the same volume to multiple containers within the same Pod.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/c1e7c110-b57b-4bff-bcf0-5836e537f618)


### Common Volume Types
There are many volume types, but there are two you may want to be especially aware of.

**hostPath:** Stores data in a specified directory on the k8s node.

**emptyDir:** Stores data in a dynamically created locations on the node. This directory exists only as long as the Pod exists on the node. the directory and the data are deleted when the Pod is removed. This volume type is very useful for simply sharing data between containers in the same Pod.

[S10-L02 Using K8s Volumes.pdf](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/files/11578788/S10-L02.Using.K8s.Volumes.pdf)



Exploring k8s Persistent Volumes
===

### PersistentVolumes
PersistentVolumes are k8s objects that allow you to treat storage as an abstract resource to be consumed by Pods, much like k8s treats compute resources such as memory and CPU.

A Persistentvolume uses a set of attributes to describe the underlying storage resource (such as a disk or cloud storage location) which will be used to store data.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/38587510-935e-4533-bb19-f2d329f8617a)


### Storage Classes

Storae Classes allow k8s administrators to specify the types of storage services they offer on their platform.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/19fa9cf6-72e8-499a-8b85-1137965a6c28)

For example, an administrator could create a StorageClass called slow to describe low-performance but inexpensive storage resources, and another called fast for high-performance but more costly resources. This would allow users to choose storage resources that fit the needs of their applications.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/dbf044b8-9b90-407f-9373-958c6eace659)


### allowVolumeExpansion
The allowVolumeExpansion property of a StorageClass determines whether or not the StorageClass supports the ability to resize volumes after they are created. If this property is not set to true, attempting to resize a volume that uses this StorageClass will result in an error.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/6e2e8728-a17f-435e-a5b8-0f48fdc13a9e)


### reclaimPolicies
A persistentVolumeReclaimPolicy determines how the storage resources can be used when the PersistentVolumes associated PersistentVolumeClaims are deteled.

#### Retain:
Keeps all data. This requires an administrator to manually clean up the data and prepare the storage resource for reuse.

#### Delete:
Deletes the underlying storage resource automatically (anly works for cloud storage resources).

#### Recycle:
Automatically deletes all data in the underlying storage resource, allowing the PersistentVolume to be reused.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/1b31a927-d6ce-4268-a9c6-e07f3aa700f8)


### PersistentVolumeClaims
A PersistentVolumeClaim represents a users request for a storage resources. It defines a set of attributes similar to those of a PersistentVolume (StorageClass, etc.)

When a PersistentVolumeClaim is created, it will look for a PersistentVolume that is able to meet the requested criteria. If it finds one, it will automatically be bound to the PersistentVolume.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/dab94f2f-f481-4b6d-a5e9-fb07ee54dabe)


### Using a PersistentVolumeClain in a Pod
PersistentVolumeClaims can be mounted to a Pods containers just like any other volume. If the PersistentVolumeClaim is bound to a PersistentVolume, the containers will use the underlying PersistentVolume storage.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/b757769b-b609-4140-9943-743310b15c89)


### Resizing a PersistentVolumeClaim
You can exand PersistentVolumeClaims without interrupting applications that are using them. Simply edit the **spec.resources.requests.storage** attribute of an existing PersistentVolumeClaim, increasing its value.

However, the StorageClass must support resizing volumes and must have **allowVolumeExpansion** set to true.

![image](https://github.com/rodolfoalmeid/Kubernetes-CKA-Study/assets/113181949/af18be1f-8f41-4e27-a16f-3f02d5719c13)
