<!-- @format -->

# non managed multi-k8s and CI/CD

how I created this cluster and integrate Github Action

docker run --name nginx-load-balancer -p 80:80 -d nginx-lb

1.  Create a VPC
    - create Public Subnet
      - Edit subnet settings
        - enable public IP
        - enable NAT
    - Internet Gateways
      - attach VPC with Internet Gateway
    - Route table
      - Edit subnet associations
      - create route to Internet Gateway
2.  Create four EC2 instances.
    - Choose AMI
    - Choose Instance Type
    - configure instance
    - Add storage
    - create a security group
    - create a key pair
    - Each instance should have a public IP address.
3.  Set up Kubernetes on each EC2 instance.
    - set security group rules
      - Edit inbound rules
    - one intance for Master Node
    - two instance for Worker Node
4.  Create a load balancer with EC2.
5.  Github Action add self hosted runner
6.  deploy the application on the Master Node

# Create a VPC

I create a VPC and I add CIDR 20.0.0.0/16

### VPC IP Address

**VPC: IP Address (20.20.0.0/16)**

<a href="./images/create-vpc.png"><img src="./images/create-vpc.png" alt="create-vpc" border="0" width="100%"></a>

## create Public Subnet

**Subnet: IP Address (20.20.1.0/24)**

<a href="./images/create-public-subnet.png"><img src="./images/create-public-subnet.png" alt="create-public-subnet" border="0" width="100%"></a>

## Edit subnet settings

- ### enable public IP

- ### enable _RBN_

<a href="./images/edit-subnet-settings.png"><img src="./images/edit-subnet-settings.png" alt="edit-subnet-settings.png" border="0" width="100%"></a>

## Internet Gateways

<a href="./images/create-igw.png"><img src="./images/create-igw.png" alt="create-igw.png" border="0" width="100%"></a>

## attach VPC with Internet Gateway

<a href="./images/attach-vpc.png"><img src="./images/attach-vpc.png" alt="attach-vpc.png" border="0" width="100%"></a>

# Route table

<a href="./images/create-route-table.png"><img src="./images/create-route-table.png" alt="create-route-table.png" border="0" width="100%"></a>

## Edit subnet associations

<a href="./images/edit-subnet-associations.png"><img src="./images/edit-subnet-associations.png" alt="edit-subnet-associations.png" border="0" width="100%"></a>

# create route to Internet Gateway

<a href="./images/create-route-to-Internet-gateway.png"><img src="./images/create-route-to-Internet-gateway.png" alt="create-route-to-Internet-gateway.png" border="0" width="100%"></a>

# Create four EC2 instances.

_we use ubuntu ec2 instance_

### Choose AMI

<a href="./images/choose-ami.png"><img src="./images/choose-ami.png" alt="choose-ami.png" border="0" width="100%"></a>

### Choose Instance Type

<a href="./images/choose-instance-type.png"><img src="./images/choose-instance-type.png" alt="choose-instance-type.png" border="0" width="100%"></a>

### configure instance

<a href="./images/configure-instance.png"><img src="./images/configure-instance.png" alt="configure-instance.png" border="0" width="100%"></a>

### Add storage

<a href="./images/add-storage.png"><img src="./images/add-storage.png" alt="add-storage.png" border="0" width="100%"></a>

### create a key pair and last part of the instance

<a href="./images/review.png"><img src="./images/review.png" alt="review.png" border="0" width="100%"></a>

# Set up Kubernetes on each EC2 instance.

### set security group rules

- ### Edit inbound rules

<a href="./images/edit-inbound-rules.png"><img src="./images/edit-inbound-rules.png" alt="edit-inbound-rules.png" border="0" width="100%"></a>

ssh your master node

    ssh -i ./devops-key-1.pem ubuntu@YOUR_PUBLIC_IP_ADDRESS

ssh your worker-1 node

    ssh -i ./devops-key-1.pem ubuntu@YOUR_PUBLIC_IP_ADDRESS

ssh your worker-2 node

    ssh -i ./devops-key-1.pem ubuntu@YOUR_PUBLIC_IP_ADDRESS

### after you install this package

[install k8s ](https://gist.github.com/hamzazahidulislam/2c7e26b70b4988be08051ff9be49566b)

# one intance for Master Node

```
sudo apt-get update && sudo apt-get upgrade -y
```

_--------- Adding Kubernetese Repository -----_

```
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
```

```
sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```

```
sudo vim /etc/apt/sources.list.d/kubernetes.list
```

```
deb http://apt.kubernetes.io/ kubernetes-xenial main
```

```
sudo apt-get update
```

_----- Installing Kubeadm ----_

```
sudo apt-get install -y kubelet=1.20.2-00 kubeadm=1.20.2-00 kubectl=1.20.2-00
```

**The kubelet is now restarting every few seconds, as it waits in a crashloop for kubeadm to tell it what to do.**

```
sudo apt-mark hold kubelet kubeadm kubectl

```

**because it swap mem can degrade K8s orchestration performance**

```
sudo swapoff -a
```

_this command run on master node only_

```
sudo kubeadm init
```

_this command you get from master node and apply all worker nodes_

```
kubeadm join 20.0.1.178:6443 --token 6k8f6s.i4maop92yoyz7f2i \
    --discovery-token-ca-cert-hash sha256:ff68e8deca05dc88326d5e2e5d23907c41e1dba61eac753ebf3e44967cde6b07
```

_this command run on master node only_

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

**_To achieve Overlay Network inside the K8s Cluster, we are going to install WeaveNet on Master node._**

```
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```
