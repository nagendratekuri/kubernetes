# How to increase connection reset timeout on Amazon Linux machines?

```sh
AWS EC2 instances will be disconnected due to inactivity. 
Inorder to keep your connection alive, edit the ssh config on the client from where you are trying to connect

Add a "client-alive" directive to the instance's SSH-server configuration file.

ClientAliveInterval 60
```

```sh
location

~/.ssh/config
```

Example:

```sh
Host kops
    HostName ec2-52-57-49-139.eu-central-1.compute.amazonaws.com
    User ec2-user
    Identityfile k8s.pem
    ForwardAgent yes
    ServerAliveInterval 60
    ServerAliveCountMax 2

```