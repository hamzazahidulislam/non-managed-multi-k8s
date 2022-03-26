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
    - Each instance should have a public IP address.
    - create a security group
    - create a key pair
3.  Create a load balancer with EC2.
4.  Set up Kubernetes on each EC2 instance.
    - one intance for Master Node
    - two instance for Worker Node
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

_we use ubuntu ec2 instances_
