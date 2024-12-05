# Mini Project: OpenStack Environment with CAPI and Kamaji
## Overview
This project aims to set up a test OpenStack environment using DevStack, configure a Cluster API (CAPI) management cluster, and create a workload Kubernetes cluster using OpenStack as the infrastructure provider and Kamaji as the control plane provider. Additionally, automation of the CAPI management cluster creation is done using argocd.
## Prerequisites
- Fresh installation of a supported Linux distribution (Ubuntu)
- Management kuberenetes cluster to run centralised tools and workload control planes(kind or docker-desktop or any production grade cluster)
- A basic installation of argocd cluster
- Git installed on your machine
- Access to a machine with sufficient resources to run OpenStack and Kubernetes clusters
- OpenStack CLI tools installed
- kubectl CLI tool installed 
## Steps
### 1. Set Up a Test OpenStack Environment Using DevStack
### 2. Apply the seed argocd manifest. 
#### We are using app of apps deployment model from argocd. Update the server_url of your management kubernetes cluster.
```
   export server_url="https://kubernetes.default.svc" 
   export K8S_MGMT_ENDPOINT=<your_mgmt_cluster_endpoint_here
   git clone 'https://github.com/vijay-dcrust/capi-kamaji-project/'
   cd capi-kamaji-project
   sed -i 's/$server_url/$K8S_MGMT_ENDPOINT/g' deploy/argocd/app-of-apps.yaml
   kubectl apply -f deploy/argocd/app-of-apps.yaml
 ```
####Applying the above manifest would take care of your entire installation and below steps (3,4 and 5) as well.
### 3. Install CAPI Management Cluster using argocd
1. Create an application using argocd to install capi management cluster.
   argocd application automatically installs the capi using the manifest from the folder manifest/capi/
   
   You can manually install using kubectl
```
    kubectl apply -f manifest/capi/
```    
### 4. Install kamaji using argocd  
1. Create an application using argocd to install kamaji on management cluster.
   argocd application automatically installs the capi using the manifest from the helm repository.
   
   You can manually install using helm as well.
```
    helm install kamaji clastix/kamaji -n kamaji-system --create-namespace
```
### 5. Create a Workload Cluster Using OpenStack and Kamaji through argocd
1. Create an application using argocd to install kamaji on management cluster.
   argocd application automatically creates a workload cluster tenant-00 using the manifest from the folder manifest/workload. You can add more manifest to this folder to create more clusters.
   
   You can manually install using kubectl
```
    kubectl apply -f manifest/workload/tcp-control-plane.yaml
```
### 6. Verify the workload cluster control plane access
```
$ k get svc -n tenant                                                                                                  [5/12/24 | 4:40:29]
NAME        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)             AGE
tenant-00   ClusterIP   10.96.188.106   <none>        6443/TCP,8132/TCP   4d1h

$ kubectl get secret tenant-00-admin-kubeconfig -n tenant -o json|jq -r '.data["admin.conf"]'|base64 --decode > tenant.kubeconfig

```
#### If you are not using a load balancer to expose your k8s control endpoint, you need to do port forwarding to access service. 
#### In this case you need to update tenant.kubeconfig with local host rather than service IP. 

<img width="586" alt="image" src="https://github.com/user-attachments/assets/209fb049-ba1e-46f5-bdc2-cf18dc4c60eb">

```
$ k get svc --kubeconfig=tenant.kubeconfig -A                                                                          [5/12/24 | 4:41:53]
NAMESPACE     NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
default       kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP                  4d1h
kube-system   kube-dns     ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   4d1h

```
## Contributing
Feel free to open issues or submit pull requests for improvements and additional features.
