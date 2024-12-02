# Mini Project: OpenStack Environment with CAPI and Kamaji
## Overview
This project aims to set up a test OpenStack environment using DevStack, configure a Cluster API (CAPI) management cluster, and create a workload Kubernetes cluster using OpenStack as the infrastructure provider and Kamaji as the control plane provider. Additionally, automation of the CAPI management cluster creation is included as a bonus.
## Prerequisites
- Fresh installation of a supported Linux distribution (Ubuntu)
- management kuberenetes cluster to run centralised tools and workload control planes(kind or docker-desktop or any production grade cluster)
- Basic knowledge of Kubernetes and OpenStack
- Git installed on your machine
- Access to a machine with sufficient resources to run OpenStack and Kubernetes clusters
- OpenStack CLI tools installed
- kubectl CLI tool installed
## Steps
### 1. Set Up a Test OpenStack Environment Using DevStack
### 2. Set Up an argocd cluster to automate the CAPI and kamaji installation
### 3. Install CAPI Management Cluster using argocd
1. Create an application using argocd to install capi management cluster.
   argocd application automatically installs the capi using the manifest from the folder manifest/capi/
   
   You can manually install using kubectl
bash
    kubectl apply -f manifest/capi/
    
### 4. Install kamaji using argocd  
1. Create an application using argocd to install kamaji on management cluster.
   argocd application automatically installs the capi using the manifest from the helm repository.
   
   You can manually install using helm
bash
    helm install kamaji clastix/kamaji -n kamaji-system --create-namespace

### 5. Create a Workload Cluster Using OpenStack and Kamaji through argocd
1. Create an application using argocd to install kamaji on management cluster.
   argocd application automatically creates a workload cluster tenant-00 using the manifest from the folder manifest/workload. You can add more manifest to this folder to create more clusters.
   
   You can manually install using kubectl
bash
    kubectl apply -f manifest/workload/

## Contributing
Feel free to open issues or submit pull requests for improvements and additional features.
