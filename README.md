# A Mini Minikube Project


<img src="logos/minikube-logo.png" alt="minikube-logo" width="150" class="center">
<img src="logos/k8s-logo.png" alt="k8s-logo" width="250" class="center">

## Requirements
* Docker installed
* kubectl
* minikube

## Starting a cluster

After requirement instalation, we start a minikube cluster by typing

`minikube start --vm-driver=docker --kubernetes-version=v1.19.0`

It going to pulling base image from minikube with **kubelet**, **kubectl** and **kubeadm** installed. Also with a default namespace. 

`minikube status`

![minikube-status](screenshots/minikube-status.png)
<br>
<br>

By now we have access to kubectl and all resources that he provide, 

`kubectl get nodes`

![kubectl-get-nodes](screenshots/kubectl-get-nodes.png)


`kubectl namespaces`

![kubectl-namespaces](screenshots/kubectl-namespaces.png)
<br>
<br>

## Main Kubectl Commands

`kubectl get service`<br>
![kubectl-get-service`](screenshots/kubectl-get-service.png)


Creating a deployment:

`kubectl create deployment --image`<br>
or<br>
`kubectl apply -f file.yml`<br>



`kubectl get pod`<br>
`kubectl get pod`<br>
`kubectl get pod`<br>