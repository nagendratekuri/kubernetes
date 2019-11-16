# We will verify our K8s cluster by deploying NGINX


### 
```sh
NGINX is used by over 40% of the world’s busiest websites and is an open-source 
reverse proxy server, load balancer, HTTP cache, and web server.  
The official image on Docker Hub has been pulled over 3.4 million times and is 
maintained by the NGINX team and the NGINX image exposes ports 80 and 443 in the container.
```

### From your master node kubectl create an nginx deployment:
```sh
kubectl create deployment nginx --image=nginx
```
### This creates a deployment called nginx. kubectl get deployments lists all available deployments:
```sh
kubectl get deployments
```
### Use kubectl describe deployment to view more information
```sh
kubectl describe deployment nginx

The describe command allows you to interrogate different kubernetes resources 
such as pods, deployments, and services at a deeper level. 
The output above indicates that there is a deployment called nginx within 
the default namespace. 
This deployment has a single replicate, and is running the docker image nginx. 
The ports, mounts, volumes and environmental variable are all unset.
```
### Make the NGINX container accessible via the internet
```sh
kubectl create service nodeport nginx --tcp=80:80

This creates a public facing service on the host for the NGINX deployment. 
Because this is a nodeport deployment, kubernetes will assign this service 
a port on the host machine in the 32000+ range.
```
### Try to get the current services
```sh
kubectl get svc
or 
kubectl get services
```
### Verify that the NGINX deployment is successful by using curl on the slave node
```sh
from the master, issue the below command

curl {name_of_any_salve}:{exposed_port_number}

Eg: curl ip-172-20-85-50.eu-central-1.compute.internal:31278

Slave node name can be found by using
kubectl get nodes

port numbber can be found by using
kubectl get svc

The output will show the unrendered “Welcome to nginx!” page HTML.
```
### To remove the deployment, use kubectl delete deployment
```sh
kubectl delete deployment nginx

Verify by using

kubectl get deployments
```
### You can also delete created services using kubectl delete svc
```sh
kubectl delete svc nginx

Verify by using

kubectl get svc
```
