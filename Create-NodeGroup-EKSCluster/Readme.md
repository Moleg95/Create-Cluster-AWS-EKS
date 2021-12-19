# Step-1: Create EKS Cluster using eksctl

   - This operation take 15 to 20 minutes to create the Cluster Control Plane:
   
	# eksctl create cluster --name=eksdemo1 --region=us-east-1 \
                    --zones=us-east-1a,us-east-1b --without-nodegroup 
	# eksctl get clusters

# Step-2: Create & Associate IAM OIDC Provider for our EKS Cluster

   - To enable and use AWS IAM roles for Kubernetes service accounts on our EKS cluster, we must create & associate 
     OIDC identity provider. Use latest eksctl version.
     
	# eksctl utils associate-iam-oidc-provider --region region-code --cluster <cluter-name> \
          --approve
   - Replace with region & cluster name
   
	# eksctl utils associate-iam-oidc-provider --region us-east-1 --cluster eksdemo1 \
          --approve

# Step-3: Create EC2 Keypair
   - Create a new EC2 Keypair with name as "kube-demo"
   - This keypair we will use it when creating the EKS NodeGroup.
   - This will help us to login to the EKS Worker Nodes using Terminal.

# Step-4: Create Node Group with additional Add-Ons in Public Subnets
   - Create Public Node Group   
	# eksctl create nodegroup --cluster=eksdemo1 --region=us-east-1 \
                       --name=eksdemo1-ng-public1 --node-type=t3.medium \
                       --nodes=2 --nodes-min=2 --nodes-max=4 \
                       --node-volume-size=20 \
                       --ssh-access \
                       --ssh-public-key=kube-demo \
                       --managed \
                       --asg-access \
                       --external-dns-access \
                       --full-ecr-access \
                       --appmesh-access \
                       --alb-ingress-access 

# Step-5: Verify Cluster & Nodes

 ## Verify NodeGroup subnets to confirm EC2 Instances are in Public Subnet
   - Go to "Services" -> "EKS" -> "eksdemo" -> eksdemo1-ng1-public
   - Click on Associated subnet in Details tab
   - Click on Route Table Tab.
     We should see that internet route via Internet Gateway (0.0.0.0/0 -> igw-xxxxxxxx)
     
 ## Verify Cluster, NodeGroup in EKS Management Console
   - Go to "Services" -> "Elastic Kubernetes Service" -> eksdemo1
   - List Worker Nodes
	# eksctl get cluster
	# eksctl get nodegroup --cluster=<clusterName>
	#kubectl get nodes -o wide
 Our kubectl context should be automatically changed to new cluster 
	# kubectl config view --minify
 
 ## Verify Worker Node IAM Role and list of Policies
   - Go to Services -> EC2 -> Worker Nodes
   - Click on "IAM Role associated to EC2 Worker Nodes"
 
 ## Verify Security Group Associated to Worker Nodes
   - Go to "Services" -> "EC2" -> Worker Nodes
   - Click on "Security Group" associated to EC2 Instance which contains 'remote' in the name.
Login to Worker Node using Keypai "kube-demo"
   - Login to worker node
  
	# ssh -i kube-demo.pem ec2-user@<Public-IP-of-Worker-Node>

# Step-6: Update Worker Nodes Security Group to allow all traffic
   - allow "All Traffic" on worker node security group

Additional References:

https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html
https://docs.aws.amazon.com/eks/latest/userguide/create-service-account-iam-policy-and-role.html

