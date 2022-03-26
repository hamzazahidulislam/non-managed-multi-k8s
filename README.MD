<!-- @format -->

# non managed multi-k8s and CI/CD

how I created this cluster and integrate Github Action

docker run --name nginx-load-balancer -p 80:80 -d nginx-lb

1.  Create a VPC
    - create Public Subnet
    - Internet Gateways
    - Route table
2.  Create four EC2 instances.
    - Each instance should have a public IP address.
    - create a security group
    - create a key pair
3.  Create a load balancer with EC2.
4.  Set up Kubernetes on each EC2 instance.
    - 
