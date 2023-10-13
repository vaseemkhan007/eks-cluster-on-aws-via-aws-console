Here are the steps to create and condigure EKS Cluster on AWS via Console
# EKS Cluster Creation on AWS Cloud via AWS Console
# 1. Prerequisite - Create two Service Role
1. eks-demo1-cluster-role and assign the below permissions
    1) AmazonEKSClusterPolicy
2. eks-demo1-node-group-role and assign the below permissions
    1) AmazonEKSWorkerNodePolicy
    2) AmazonEC2ContainerRegistryReadOnly
    3) AmazonEKS_CNI_Policy
 
It is not easy to discribe what are the above policies in this documents, you can [here](https://docs.aws.amazon.com/eks/latest/userguide/service_IAM_role.html#create-service-role) read for more details

# 2. How to create the policies- 
Open the IAM [console](https://console.aws.amazon.com/iam)

    1) Choose Roles, then Create role.
    2) Under Trusted entity type, select AWS service.
    3) From the Use cases for other AWS services dropdown list, choose "EKS".
    4) Choose "EKS - Cluster" for your use case, and then choose Next.
    5) On the Add permissions tab, choose Next.
    6) For Role name, enter a unique name for your role, such as "eks-demo1-cluster-role".
    7) For Description, enter descriptive text such as "KS Cluster Role".
    8) Choose Create role.

Now create the *eks-demo1-node-group-role* policy and attach the permissions described as below:

Open the IAM [console](https://console.aws.amazon.com/iam/)

    1) In the left navigation pane, choose Roles.  
    2) On the Roles page, choose Create role.  
    3) On the Select trusted entity page, do the following:  
    4) In the Trusted entity type section, choose AWS service.  
    5) Under Use case, choose EC2.  


# 3. Cluster Creation Process  
Go to the EKS Console [home](https://us-east-2.console.aws.amazon.com/eks/home?region=us-east-2#/home) page 

EKS>Clusters>Create EKS Cluster

    Step 1 Configure cluster - Provide Name(eks-demo1-cluster), Version and choose Cluster Service Role eks-demo1-cluster-role
    Step 2 Specify networking - Choose default configurations under "Specify Networking" section then click Next, though if you 
    can choose your own created VPC, Security Group if you want, though by default it will create it self and you can choose
    default VPC as well
    Step 3 Configure logging - Don't enable any logging to save cost then click Next
    Step 4 Select add-ons - Select default addons then click next
    Step 5 Configure selected add-ons settings
    Step 6 Review and create

# 4. Add node group to your cluster  
Go to the created cluster's compute tab and Click "Add node group"

    Step 1 Configure node group - Choose Name(eks-demo1-node-group), keep other default
    Step 2 Set compute and scaling configuration - Choose, AMI, Compute Capacity, Scaling etc (you can keep the default)
    Step 3 Specify networking - (Can keep default)
    Step 4 Review and create - You can configure remote access to nodes and provide (key pairs for these ec2's)  

It will take time to create the desired node group with desired (mentioned capacity)

# 5. Connect to Cluster  

Your laptop or EC2 machine on the same VPC/Network can be used to connect to the cluster. You need to [install](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html) "kubeclt" to execute the commands on the created cluster

Execute the below command to add contex to kube config
~~~
aws eks update-kubeconfig --region us-east-2 --name eks-demo1-cluster  

[ec2-user@ip-10-0-1-129 ~]$ kubectl get nodes
NAME                                         STATUS   ROLES    AGE   VERSION
ip-10-0-136-64.us-east-2.compute.internal    Ready    <none>   12m   v1.28.1-eks-43840fb
ip-10-0-157-222.us-east-2.compute.internal   Ready    <none>   12m   v1.28.1-eks-43840fb
~~~
