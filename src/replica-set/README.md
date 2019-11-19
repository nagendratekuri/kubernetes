# Replica Set

### Description
```sh
Replication Controller helps us run multiples instances of a single POD in the Kubernetes cluster, 
thus providing high availablity.

Another reason we need replication controller is to create multiple PODs for 
load balancing and scaling.

Replication Controller spans across multiple nodes in the cluster.

Replication Controller vs Replica Set:

Both are designed for the same purpose but Replica Set is new 
and the Replication Controller is deprecated.

The main difference is ReplicaSet has a mandatory property 'selector'. It is because ReplicaSet can also manage
the PODs that are not created as part of the ReplicaSet

To create more replicas: 

1st Option: 
 Change the value replicas and use 
 kubectl replace -f rs-definition.yml
 
2nd Option:
 kubectl scale --replicas=6 rs-definition.yml
 

```

### commands
```sh
kubectl create -f rc-definition.yml
kubectl create -f rs-definition.yml

kubectl get replicationcontroller
kubectl get replicatset

kubectl delete replicaset myapp-rs

kubectl get pods
```