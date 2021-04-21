# A Mini Minikube Project


<img src="logos/minikube-logo.png" alt="minikube-logo" width="150" class="center">&nbsp;&nbsp;
<img src="logos/k8s-logo.png" alt="k8s-logo" width="250" class="center">

## Requirements
* Docker installed
* kubectl
* minikube

## Starting a cluster

After requirement instalation, we start a minikube cluster by typing

`minikube start --vm-driver=docker --kubernetes-version=v1.19.0`

It going to pulling base image from minikube with **kubelet**, **kubectl** and **kubeadm** installed. Also with a default namespace. 

`minikube status`<br>
![minikube-status](screenshots/minikube-status.png)
<br>
<br>

By now we have access to kubectl and all resources that he provide, 

`kubectl get nodes`<br>
![kubectl-get-nodes](screenshots/kubectl-get-nodes.png)


`kubectl namespaces`<br>
![kubectl-namespaces](screenshots/kubectl-namespaces.png)
<br>
<br>

## Main Kubectl Commands

`kubectl get service`<br>
![kubectl-get-service`](screenshots/kubectl-get-service.png)

<br>

Creating a deployment:

`kubectl create deployment nginx-deploy --image nginx:latest --replicas=3 --port=80`<br>
or<br>
`kubectl apply -f deployments/nginx.yml`<br>
![kubectl-apply`](screenshots/kubectl-apply.png)


`kubectl get deployments`<br>
![kubectl--get-deployments`](screenshots/kubectl-get-deployments.png)


`kubectl get pods`<br>
`kubectl get pods --namespace default`<br>
![kubectl-get-pods`](screenshots/kubectl-get-pods.png)


`kubectl get replicaset`<br>
![kubectl-get-replicaset`](screenshots/kubectl-get-replicaset.png)

<br>

Debug commands:

`kubectl describe pod [pod name]`<br>

`kubectl logs [pod name]`<br>
![kubectl-logs`](screenshots/kubectl-logs.png)

Get inside pod:

`kubectl exec -it [pod name] -- /bin/bash`<br>
![kubectl-exec`](screenshots/kubectl-exec.png)

<br>

`kubectl delete deployment nginx-deploy`<br>
![kubectl-delete-deployment`](screenshots/kubectl-delete-deployment.png)

