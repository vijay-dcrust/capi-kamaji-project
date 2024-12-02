# Mini Project: OpenStack Environment with CAPI and Kamaji
## Overview
This project aims to set up a test OpenStack environment using DevStack, configure a Cluster API (CAPI) management cluster, and create a workload Kubernetes cluster using OpenStack as the infrastructure provider and Kamaji as the control plane provider. Additionally, automation of the CAPI management cluster creation is included as a bonus.
## Prerequisites
- Fresh installation of a supported Linux distribution (Ubuntu or CentOS recommended)
- Basic knowledge of Kubernetes and OpenStack
- Git installed on your machine
- Access to a machine with sufficient resources to run OpenStack and Kubernetes clusters
- OpenStack CLI tools installed
- kubectl CLI tool installed
## Steps
### 1. Set Up a Test OpenStack Environment Using DevStack
### 2. Set Up a CAPI Management Cluster
1. Install the clusterctl CLI tool:
   Follow the instructions for your operating system at the official Cluster API documentation: https://cluster-api.sigs.k8s.io/user/quick-start.html
2. Initialize the management cluster:
   
bash
    clusterctl init --infrastructure openstack
    
   This sets up the Cluster API components in your management cluster and installs the OpenStack infrastructure provider.
### 3. Create a Workload Cluster Using OpenStack and Kamaji
## Contributing
Feel free to open issues or submit pull requests for improvements and additional features.
