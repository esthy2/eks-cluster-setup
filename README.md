[Intro | 0:00 – 0:30]
“Hey everyone, welcome back to EsthyTech! In today’s class, we’re diving into one of the most powerful ways to run Kubernetes in production. Amazon EKS. We’ll be setting up a Kubernetes cluster using eksctl, a super simple CLI tool that makes the whole process smooth and fast.
By the end of this session, you’ll have your very own production-ready EKS cluster up and running, and you’ll be able to start deploying workloads right away. So grab your coffee, and let’s get started!”

## Setup EKS cluster using eksctl
### Overview
There are various ways to setup kubernetes cluster. For this class, we will setup a kubernetes cluster in EKS using eksctl.
This is the production ready cluster we will use throughout the lab.

### Prerequisites
Before proceeding, ensure that you have the following prerequisites:

- **AWS Account:** You need to have an AWS account with the appropriate permissions to create and manage EKS clusters.

- **IAM Permissions:** Ensure that your IAM user has sufficient permissions to create an EKS cluster, EC2 instances, and other resources required by EKS. An administrator account works well!

- **AWS CLI:** The AWS Command Line Interface (CLI) must be installed and configured (`aws configure`) with your AWS credentials (access key, secret key, region ...).
- **kubectl installed:** (via choco on Windows or brew on Mac).

- **eksctl installed:** (again, choco or brew makes it easy).
**Make sure these are in place before moving forward**

### Install kubectl

**kubectl:** is the Kubernetes command-line tool used to interact with the cluster. You can install it following [the instructions here](https://kubernetes.io/docs/tasks/tools/#kubectl).

**Note:** You can use ``choco`` on windows or ``brew`` on Mac

**1. On Windows with choco**
```bash
choco install kubernetes-cli
```

After successful installation, run the following command to verify the version installed:
```bash
kubectl version --client
```

**2. On Mac with Homebrew**

```bash
brew install kubectl
```
or
```bash
brew install kubernetes-cli
```
After successful installation, run the following command to verify the version installed:
```bash
kubectl version --client
```

### Install eksctl

**eksctl** is a simple CLI tool for creating and managing clusters on EKS.
#### Install eksctl on Windows
Open Powershell as administrator and install eksctl using choco with the command:
```bash
choco install eksctl
```

#### Install eksctl on MAC
You can use Homebrew to install eksctl with the command:
 ```bash
brew tap weaveworks/tap
brew install weaveworks/tap/eksctl
```
**Don’t forget to verify the versions with ```bash kubectl version --client ``` and upgrade eksctl if you run into version issues.**
Refer to the following documentation to install eksctl on your system if the above commands do not work for you: [Link here](https://eksctl.io/installation/)

### Create the cluster in EKS

Now comes the fun part: creating our cluster. Run this command:
```bash
eksctl create cluster --name my-cluster --region us-east-1 --nodegroup-name my-nodes --node-type t3.small --nodes 2 --nodes-min 1 --nodes-max 2
```
**Note:** This command will take about 10–15 minutes, depending on your computer and network connection. Be patient and wait till the end of the process

 **WARNING:** If you see errors like `Error: invalid version, supported values: 1.19, 1.20, 1.21, 1.22 ...`, you need to upgrade your eksctl version. On Windows, run `choco upgrade eksctl`, or `brew upgrade eksctl` on Mac.

 **WARNING:** If you get an error `AlreadyExistsException: Stack[...]...` in your terminal when launching the cluster, just login to aws and delete the corresponding CloudFormation stack. You might need to forcefully delete it with the option **Force delete the entire stack**.

### Update the Kubeconfig

After creating the cluster, the local kubeconfig file is automatically upated with your cluster information. No need to update the context, you can start to interact with the cluster.

Verify that you can access the cluster. You should see nodes READY.
```bash
kubectl get nodes
```

Note: If the previous throws and error, then update the kubeconfig with the command and retry:

```bash
aws eks update-kubeconfig --region us-east-1 --name my-cluster
```

### Basic commands
These commands help you navigate and monitor your cluster.

| **Command**                                      | **Description**                                                                 |
|--------------------------------------------------|---------------------------------------------------------------------------------|
| `kubectl cluster-info dump`                      | Get details about the cluster (API server, components, etc.).                   |
| `kubectl config view`                            | view the Kubeconfig file                   |
| `kubectl get nodes`                              | List all nodes in the Kubernetes cluster.                                       |
| `kubectl get nodes -o wide`                      | Show more details on node                                  |
| `kubectl get pods`                               | List all pods in the default namespace.                                        |
| `kubectl get pods -o wide`                       | Show more details (e.g., pod IP, node, etc.).                                   |
| `kubectl get deployments`                        | List all deployments in the default namespace.                                  |
| `kubectl get services`                           | List all services in the defaut namespace                                            |
| `kubectl proxy`                                  | Access the Kubernetes cluster dashboard (if set up).                            |

## Delete the EKS Cluster

When you’re done practicing, delete the cluster to avoid costs. Run this command only when you are done practicing 
```bash
eksctl delete cluster --name my-cluster --region us-east-1
```
Always clean up your resources to save money in AWS
After deleting the cluster, you can verify in Cloudformation that all the stacks related to the cluster creation were successfully deleted.

Outro | 6:30 – End]
“And that’s it — you’ve just created, verified, and even cleaned up a Kubernetes cluster on Amazon EKS using eksctl! 🎉

## In the next video, we’ll dive deeper into deploying workloads on this cluster and managing them effectively.
If you found this tutorial helpful, please smash that Like button, Subscribe to EsthyTech, and hit the bell icon so you don’t miss the next lesson. And hey, drop your questions in the comments — I’d love to hear how your cluster setup went.
Thanks for watching, and as always… keep learning with Esthytech!


## COMING UP (Optional) Cluster Setup with Terraform



