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
kubectl tool and k8s Control Plane v1.36.0 with Cilium v1.19.3 and deploy Argo CD v3.39.0 and NGINX v1.30.0
Configure Argo CD to maintain expected state from https://github.com/kevinayres/kubeadm-teleport/...

Build workflow: 
A single AWS EC2 VPC was created with the default CIDR (172.31.0.0/16), Subnet, Security Group and Routing Table configuration. 
Inbound SSL and HTTPS were termporarily allowed from all external IP's. 
The cluster exists within a single network security domain to prevent the need for additional ACL's, therefore a single combined Security Group, rather than separate groups for control plane and worker nodes is implemented. 
3x EC2 instances meeting the minimum hardware requirements for a k8s install were deployed into a single Availability Zone for simplicity and without regard for reliability. 
<img width="3200" height="1490" alt="image" src="https://github.com/user-attachments/assets/378631e1-4b1f-46df-a232-407e36df7c03" />
<img width="2942" height="488" alt="image" src="https://github.com/user-attachments/assets/c8a072e9-68cb-434f-9262-110713b4dbf2" />

Provision nodes

<img width="1684" height="1244" alt="image" src="https://github.com/user-attachments/assets/441b92ed-7217-4e58-a16a-234e605e93ec" />

→ Prepare Linux OS
→ Install containerd runtime
→ Install kubelet, kubeadm, kubectl
→ Initialize control plane with kubeadm
→ Configure kubectl access
→ Install CNI networking
→ Join worker nodes
→ Validate cluster
→ Add storage, ingress, monitoring, security, and apps
Install helm, kubectl, kubeadm, kubelet, containerd across nodes as appropriate.

REFERENCES: 
https://kubernetes.io/docs
https://github.com/containerd/containerd/blob/main/docs/getting-started.md
$ tar Cxzvf /usr/local containerd-2.3.0-linux-amd64.tar.gz
