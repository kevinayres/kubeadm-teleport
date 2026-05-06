# kubeadm-teleport
kubeadm on ec2 AL2 with NGINX and basic Argo CD
In this test recipe we are configuring a 3-node kubeadm cluster with 2 worker nodes running an NGINX ingress and static web site. 
k8s RBAC is configured with a User account capable of deploying and updating the web site. 
Argo CD is deployed to the cluster to maintain the desired state. 

The goal is to provide a clean customer demonstration by using a simple design while minimizing the chances of in-demo issues. 
The focus is on simplicity for a non-production environment and design decisions reflect this. Including: 
A single repo for all functions. No separation of configuration from images. 
Default naming conventions for VPC, security groups, etc. 
Public IP's for all nodes in a single layer subnet design. 
A single k8s environment without staging using latest stable bits. Including: 
kubectl tool and k8s Control Plane with Cilium, Argo CD, and NGINX.
Configure Argo CD to maintain expected state from https://github.com/kevinayres/kubeadm-teleport/...

Build workflow: 
A single AWS EC2 VPC was created with the default CIDR (172.31.0.0/16), Subnet, customer Security Group and default Routing Table configuration. 
Inbound SSH, HTTPS, and TCP 6443 were termporarily allowed from the admin workstation. 
All traffic is allowed within the VPC for build/test. 
The cluster exists within a single network security domain to prevent the need for additional ACL's, therefore a single combined Security Group, rather than separate groups for control plane and worker nodes is implemented. 
3x EC2 instances meeting the hardware requirements for a k8s install with NGINX were deployed into a single Availability Zone for simplicity and without regard for reliability. 
<img width="3200" height="1490" alt="image" src="https://github.com/user-attachments/assets/378631e1-4b1f-46df-a232-407e36df7c03" />
<img width="2960" height="700" alt="image" src="https://github.com/user-attachments/assets/94f9d33a-db9b-4271-a2a8-cd2150fe4d5f" />



Provision nodes

<img width="1634" height="324" alt="image" src="https://github.com/user-attachments/assets/1b9982c4-3632-4104-bd6d-c3e84370f7b7" />


<img width="1684" height="1244" alt="image" src="https://github.com/user-attachments/assets/441b92ed-7217-4e58-a16a-234e605e93ec" />

##VERSION COMPATABILITY
Kubernetes:        1.35.4  
kubeadm:           1.35.4  
kubelet:           1.35.4  
kubectl:           1.35.4  
containerd:        2.3.0  
Cilium:            1.19.3  
cert-manager:      1.20.2  
ingress-nginx:     controller v1.15.1 / Helm chart 4.15.1  
Static NGINX app:  nginx:stable-alpine  

##INSTALL ORDER

0. Prepare OS - hostname, Apparmor, IPtables, updates, curl, helm
1. Install containerd 2.3.0
2. Install kubeadm/kubelet/kubectl 1.35.4
3. kubeadm init # with --skip-phases=addon/kube-proxy for Cilium kube-proxy replacement
5. Install Cilium 1.19.3
6. Join worker nodes.
7. ....*Validate clean join
8. Install cert-manager 1.20.2
9. Create ClusterIssuer / self-signed CA
10. Configure RBAC for Static NGINX site
11. ....1. Create namespace, User, Role
12. ....2. Bind Role to User
13. Deploy static NGINX site as User
14. Create AWS Network Load Balancer with 2 target groups for HTTP and HTTPS passthrough to ingress-nginx
    Load Balancer DNS name: ingress-2e450c9ceaa660d3.elb.us-east-1.amazonaws.com
15. Test
16. Deploy and integrate Argo CD. 



REFERENCES: 
https://kubernetes.io/docs

https://github.com/containerd/containerd/blob/main/docs/getting-started.md

https://cert-manager.io/v1.2-docs/installation/uninstall/kubernetes/
https://cert-manager.io/docs/releases/release-notes/release-notes-1.20

https://nginx.org/en/linux_packages.html#Ubuntu
https://github.com/nginx/kubernetes-ingress 

