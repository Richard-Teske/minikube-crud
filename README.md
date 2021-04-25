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

to open dashboard (only in minikube)<br>
`minikube dashboard` 

<br>

By now we have access to kubectl and all resources that he provide, 

`kubectl get nodes`<br>
![kubectl-get-nodes](screenshots/kubectl-get-nodes.png)


`kubectl namespaces`<br>
![kubectl-namespaces](screenshots/kubectl-namespaces.png)

<br>

## Main Kubectl Commands

`kubectl get service`<br>
![kubectl-get-service](screenshots/kubectl-get-service.png)

`kubectl describe service [service name]`<br>
![kubectl-get-service](screenshots/kubectl-describe-service.png)


Creating a deployment:

`kubectl create deployment nginx-deploy --image nginx:latest --replicas=3 --port=80`<br>
or<br>
`kubectl apply -f deployments/nginx.yml`<br>
![kubectl-apply](screenshots/kubectl-apply.png)

`kubectl get deployments`<br>
![kubectl--get-deployments](screenshots/kubectl-get-deployments.png)

`kubectl get pods`<br>
`kubectl get pods --namespace default`<br>
![kubectl-get-pods](screenshots/kubectl-get-pods.png)

`kubectl get replicaset`<br>
![kubectl-get-replicaset](screenshots/kubectl-get-replicaset.png)

`kubectl delete deployment nginx-deploy`<br>
![kubectl-delete-deployment](screenshots/kubectl-delete-deployment.png)


Debug commands:<br>
`kubectl describe pod [pod name]`<br>

`kubectl logs [pod name]`<br>

Get inside pod:<br>
`kubectl exec -it [pod name] -- /bin/bash`<br>
![kubectl-exec](screenshots/kubectl-exec.png)

<br>

## MongoDB & Mongo Express services

Creating deployment and service for MongoDB:<br>
`kubectl apply -f mongodb/mongo.yml`<br>

Creating deployment and service for MongoExpress:<br>
`kubectl apply -f mongodb/mongo-express.yml`<br>

Mongo Express needs to be a **external services**. We can define the type in service yaml file, for external services we put LoadBalancer.<br>
![img-loadbalancer](screenshots/img-loadbalancer.png)

Reminder: *Internal services acts as Load Balancer too*

We also should create a **external ip address** and so accepts external requests. For external address we define as **nodePort** in services yaml file. It must be between **30000 and 32767**<br>
![img-external-port](screenshots/img-external-port.png)

As result, the services has now a LoadBalancer type and a external ip address signed: <br>
![img-loudbalancer-externalip](screenshots/img-loudbalancer-externalip.png)

But since we are working with minikube, status of EXTERNAL-IP is always pending. To fix that we should call:

`minikube service [service-name]`<br>
![minikube-service](screenshots/minikube-service.png)

This command basically assign a external ip address to our service, making it accessible.

When we send a request to Mongo Express, the External Service communicate with Mongo Express Pod, that communicate with MongoDB Internal Service, and finally he communicate with MongoDB Pod to apply request changes to MongoDB itself

Finally we have setup a simple application using Kubernetes 😁

<br>

## Ingress

Using minikube, we have to enable K8s Nginx ingress controller by typing `minikube addons enable ingress`

After that, a namespace called **ingress-nginx** is created, containing ingress controller pods, services, deployment and replicaset.

We are going to create a ingress rule to access kubernetes dashboard using a yaml file already configured (*dashbaord-ingress.yml*) <br>
`kubectl apply -f dashboard-ingress.yml`

After that:

`kubectl get ingress dashboard-ingress -n kubernetes-dashboard`

Then it will show host, ip address and port of our ingress

![kubectl-get-ingress](screenshots/kubectl-get-ingress.png)

Now we have to add this ip address to /etc/hosts file:

![etc-hosts-file](screenshots/etc-hosts-file.png)

Then typing dashboard.com in web browser should open kubernetes dashboard

### Multiple rules

You could also get multiple rules in the same host as shown below

![img-ingress-multiple-rules](screenshots/img-ingress-multiple-rules.png)

In this example, we have a same host **myapp.com** but with 2 paths configured **/analytics** and **/development**. Accessing myapp.com/analytics will send requests to *analytics-services* and the service will call analytics Pod, same happens with development rule. That's a method to have multiple rules in the same host, requesting multiple services/pods

### Sub-domains

Sub-domains are a valid method to request multiples services in kubernetes too.

![img-ingress-sub-domains](screenshots/img-ingress-sub-domains.png)

Instead having 1 host (*myapp.com*) and multiple paths (*myapp.com/analytics* and *myapp.com/development*) now we have **multiple hosts** (*analytics.myapp.com* and *development.myapp.com*) when each host represents a sub-domain

### Certification

To configure a TLS certification, we must create a secret contaning certification and key, both base64 encoded (*tls-certification-secret.yml example*). In ingress yaml file, there's a tls option where we setup the secret name defined before in secret file

![img-ingress-tls](screenshots/img-ingress-tls.png)

1. secretName key is the same that metadata.name secret file
2. data keys need to be "**tls.crt**" and "**tls.key**"
3. values from crt and key should be their **contents**, not paths
4. secret component must be in the **same namespace** as the ingress component 

These four lines in ingress file are all we have to do to setup a TLS certification 