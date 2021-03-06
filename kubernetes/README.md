# Kubernetes
Kubernetes is an open source container orchestration engine for automating deployment, scaling, and management of containerized applications.

# Kubernetes Arch

![KubernetesArch](../docs/images/kubernetes_arc.jpg)

## UI
You can use Dashboard to deploy containerized applications to a Kubernetes cluster, 
troubleshoot your containerized application, and manage the cluster resources. 
You can use Dashboard to get an overview of applications running on your cluster, as well as for creating or modifying individual Kubernetes resources

## Kubectl
kubectl controls the Kubernetes cluster manager. It's Cli for running commands to manage the cluster.

## Pods
Pods are the smallest deployable units of computing that can be created and managed in Kubernetes.

## kubelet
The kubelet is the primary “node agent” that runs on each node. The kubelet works in terms of a PodSpec. 
A PodSpec is a YAML or JSON object that describes a pod. 
The kubelet takes a set of PodSpecs that are provided through various mechanisms (primarily through the apiserver) and 
ensures that the containers described in those PodSpecs are running and healthy. 
The kubelet doesn’t manage containers which were not created by Kubernetes

## Kubernetes Master
The Kubernetes master is responsible for maintaining the desired state for your cluster. When you interact with Kubernetes, 
such as by using the kubectl command-line interface, you’re communicating with your cluster’s Kubernetes master.
The “master” refers to a collection of processes managing the cluster state. Those processes are: kube-apiserver, kube-controller-manager, kube-scheduler and etcd.
Typically these processes are all run on a single node in the cluster,
and this node is also referred to as the master. 
The master can also be replicated for availability and redundancy.

- kube-apiserver
- kube-controller-manager
- kube-scheduler
- etcd

## Kubernetes Nodes
The nodes in a cluster are the machines (VMs, physical servers, etc) that run your applications and cloud workflows. 
The Kubernetes master controls each node; you’ll rarely interact with nodes directly.

## Install kubectl
- install kubectl

        curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
        chmod +x ./kubectl
        sudo mv ./kubectl /usr/local/bin/kubectl

- Install aws-iam-authenticator

        curl -o aws-iam-authenticator https://amazon-eks.s3-us-west-2.amazonaws.com/1.12.7/2019-03-27/bin/linux/amd64/aws-iam-authenticator

## Create EKS on AWS

    aws eks create-cluster --name eksdemo --role-arn <eksRole> --resources-vpc-config subnetIds=<subnets>,securityGroupIds=<securityGrps>
    aws eks --region ap-southeast-2 create-cluster --name test --role-arn arn:aws:iam::123456789:role/aws_eks_role --resources-vpc-config subnetIds=subnet-bc61f1d8,subnet-a611a9d0,subnet-f2a100ab,securityGroupIds=sg-9279b4f5

## Create kubeconfig file
    aws eks --region region update-kubeconfig --name cluster_name
    
## For manually configuring the kubeconfig file, just need to set the environment variable with kubeconfig file as below
        export KUBECONFIG=$KUBECONFIG:/.kube/kubeconfig_eks_cluster
        
## test configuration

    kubectl get svc
    
## To enable worker nodes to join your cluster
Use the following command to download the AWS IAM Authenticator configuration map and replace the <ARN of instance role (not instance profile)> snippet with the NodeInstanceRole value:

    curl -o aws-auth-cm.yaml https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-02-11/aws-auth-cm.yaml

Apply the configuration. This command may take a few minutes to finish.

    kubectl apply -f aws-auth-cm.yaml

Check to see if you have already applied the aws-auth ConfigMap.

    kubectl describe configmap -n kube-system aws-auth

Once node registered to the cluster, use below command to validate the nodes

    kubectl get nodes --watch

Deploy a sample application to kubernetes by referring https://docs.aws.amazon.com/eks/latest/userguide/eks-guestbook.html

To scale the deployemnt just need to use scale option with replica count
        
        kubectl scale deployment <deployment_name> --replicas=3
        
Delete all services and pods running in kubernetes

    kubectl delete svc --all
    kubectl delete pods --all

## Refer the kubernetes deployment and service example

    [examples](https://github.com/mattyait/devops_learning/blob/master/kubernetes/example.md)
