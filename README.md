# Installing Kubernetes on AWS using KOPS (Kubernetes Operations)

### 1. Launch Linux EC2 instance in AWS
    This EC2 is used as a KOPS server.
### 2. Create and attach IAM role to EC2 Instance.
	Kops need permissions to access
		S3, EC2, VPC, Route53, Autoscaling, etc..
### 3. Install KOPS on EC2
```sh
sudo yum install wget
wget https://github.com/kubernetes/kops/releases/download/1.10.0/kops-linux-amd64
chmod +x kops-linux-amd64
sudo mv kops-linux-amd64 /usr/local/bin/kops

After installing, you can verify by using

kops version 
```

### 4. Install kubectl
```sh
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl

After installing, you can verify by using

kubectl version 
```
### 5 Configure environment variables.
Open .bashrc file 
```
	vi ~/.bashrc
```
Add following content into .bashrc
```sh
export KOPS_CLUSTER_NAME=nag.k8s
export KOPS_STATE_STORE=s3://nd-group.k8s
Note: Make sure this bucket is created in your AWS account.
```
Then source your bashrc to update your env variables.
```
	source ~/.bashrc
```
### 6. Create a private hosted zone in AWS Route53
 1. Go to aws Route53 and create a hostedzone
 2. Give the same name as your kops cluster name i.e. nag.k8s
 3. Choose type as privated hosted zone for VPC
 4. Select default vpc in the region you are setting up your cluster
 5. Hit create


### 7. Create ssh key pair
This keypair is used for ssh into kubernetes cluster

```sh
ssh-keygen
```

### 8. Create a Kubernetes cluster definition.
```sh
kops create cluster \
--state=${KOPS_STATE_STORE} \
--node-count=2 \
--master-size=t2.micro \
--node-size=t2.micro \
--zones=eu-central-1a,eu-central-1b \
--name=${KOPS_CLUSTER_NAME} \
--dns private \
--master-count 1

** If you don't know the zones avilable in your region, use the below command.

aws ec2 describe-availability-zones --region eu-central-1

```

### 9. Create kubernetes cluster

```sh
kops update cluster --yes
```
Just wait for some time and check the status using below command.

```sh
kops validate cluster
```
### 10. To connect to the master
```sh
ssh admin@api.nag.k8s
```
# If you want to delete the kubernetes cluster
```sh
kops delete cluster  --yes
```