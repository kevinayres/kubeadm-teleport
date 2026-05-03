# kubeadm-teleport
kubeadm on ec2 AL2 with NGINX and basic Argo CD
In this test recipe we are configuring a 3-node kubeadm cluster with 2 worker nodes running an NGINX ingress and static web site. 
k8s RBAC is configured with a User account capable of deploying and updating the web site. 
Argo CD is deployed to the cluster and leveraged for site content updates. 

The goal is to provide a clean customer demonstration by using a simple design while minimizing the chances of in-demo issues. 
The focus is on simplicity for a non-production environment and design decisions reflect this. Including: 
A single repo for all functions. 
Default naming conventions for VPC, security groups, etc. 
Public IP's for all nodes in a single layer subnet design. 
k8s v. with Cilium v. and Argo CD v. 

Build workflow: 
A single AWS EC2 VPC was created with the default Subnet, Security Group and Routing Table configuration. 
Inbound SSL and HTTPS were termporarily allowed from all external IP's. 
The cluster exists within a single network security domain to prevent the need for additional ACL's. 
3x EC2 instances meeting the minimum hardware requirements for a k8s install were deployed into a single Availability Zone for simplicity and without regard for reliability. 
