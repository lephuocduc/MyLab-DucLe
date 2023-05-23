# Get Ready for DevOps and Containers

An introduction to the principles of DevOps and containerisation using Azure DevOps and Azure Kubernetes Service. This lab borrows heavily from the excellent [Azure DevOps Hands on Labs website](https://www.azuredevopslabs.com/labs/vstsextend/kubernetes/), but adds in a bit more detail on some steps for users new to Cloud or Azure.

## What is Kubernetes and the Azure Kubernetes Service?
![image](https://github.com/lephuocduc/TheLabGuide/assets/37317309/a86cd979-3d9b-43f7-8ba6-cfac6ebb96b0)

Kubernetes is an open-source container orchestration system that automates deployment, scaling, and management of containerized applications. It is a popular choice for running containerized applications in production, as it provides a number of features that make it easy to manage and scale applications. To understand a bit more about Kubernetes, we really recommend [this video](https://www.youtube.com/watch?v=4ht22ReBjno) - it's an illustrated guide to Kubernetes and does an amazing job of distilling some advanced concepts into a short video guide.

Here is a very basic glossary of some key Kubernetes terms/concepts you'll come across in the lab, but don't worry about understanding them too much in detail at this stage as that's out of scope of this lab.
1.Node: A node is a worker machine in Kubernetes. It can be a virtual or physical machine, depending on the cluster. Each node is managed by the control plane and contains the services necessary to run Pods.

2. Cluster: A cluster is a group of nodes that are managed together by Kubernetes. A cluster can be made up of any number of nodes, and each node can have different hardware specifications.

3. Pod: A pod is a group of one or more containers that are scheduled together on a single node. Pods are the smallest unit of deployment in Kubernetes.

4. Service: A service is a logical abstraction that exposes Pods to the rest of the cluster. Services can be exposed using a variety of protocols, including HTTP, HTTPS, and TCP.

5. Storage: Kubernetes provides a variety of storage options for Pods. These options include local storage, network storage, and cloud storage.

6. Container Registry: A container registry is a repository for storing container images. Container images are used to deploy Pods in Kubernetes.

## Azure Kubernetes Service

Azure Kubernetes Service (AKS) is a managed Kubernetes service that makes it easy to deploy, manage, and scale containerized applications on Azure. AKS provides a high-level abstraction for Kubernetes, making it easy to use Kubernetes without having to worry about the underlying infrastructure.
![image](https://github.com/lephuocduc/TheLabGuide/assets/37317309/4576dc3c-60a1-48b8-8d5d-6287f9ee50ad)



# What is Azure DevOps?

Azure DevOps is a suite of tools that helps teams to plan, develop, test, and deploy software. It provides a single place for teams to collaborate on code, track work, and manage releases. 
![image](https://github.com/lephuocduc/TheLabGuide/assets/37317309/e808db0b-5815-4b21-a3a7-b8e074543bd0)


