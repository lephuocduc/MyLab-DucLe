# Get Ready for DevOps and Containers

An introduction to the principles of DevOps and containerisation using Azure DevOps and Azure Kubernetes Service. This lab borrows heavily from the excellent [Azure DevOps Hands on Labs website](https://www.azuredevopslabs.com/labs/vstsextend/kubernetes/), but adds in a bit more detail on some steps for users new to Cloud or Azure.

## What is Kubernetes and the Azure Kubernetes Service?
![image](https://github.com/lephuocduc/TheLabGuide/assets/37317309/a86cd979-3d9b-43f7-8ba6-cfac6ebb96b0)

Kubernetes is an open-source container orchestration system that automates deployment, scaling, and management of containerized applications. It is a popular choice for running containerized applications in production, as it provides a number of features that make it easy to manage and scale applications. To understand a bit more about Kubernetes, we really recommend [this video](https://www.youtube.com/watch?v=4ht22ReBjno) - it's an illustrated guide to Kubernetes and does an amazing job of distilling some advanced concepts into a short video guide.

Here is a very basic glossary of some key Kubernetes terms/concepts you'll come across in the lab, but don't worry about understanding them too much in detail at this stage as that's out of scope of this lab.
1. **Node:** A node is a worker machine in Kubernetes. It can be a virtual or physical machine, depending on the cluster. Each node is managed by the control plane and contains the services necessary to run Pods.

2. **Cluster:** A cluster is a group of nodes that are managed together by Kubernetes. A cluster can be made up of any number of nodes, and each node can have different hardware specifications.

3. **Pod:** A pod is a group of one or more containers that are scheduled together on a single node. Pods are the smallest unit of deployment in Kubernetes.

4. **Service:** A service is a logical abstraction that exposes Pods to the rest of the cluster. Services can be exposed using a variety of protocols, including HTTP, HTTPS, and TCP.

5. **Storage:** Kubernetes provides a variety of storage options for Pods. These options include local storage, network storage, and cloud storage.

6. **Container Registry:** A container registry is a repository for storing container images. Container images are used to deploy Pods in Kubernetes.

## Azure Kubernetes Service

Azure Kubernetes Service (AKS) is a managed Kubernetes service that makes it easy to deploy, manage, and scale containerized applications on Azure. AKS provides a high-level abstraction for Kubernetes, making it easy to use Kubernetes without having to worry about the underlying infrastructure.
![image](https://github.com/lephuocduc/TheLabGuide/assets/37317309/4576dc3c-60a1-48b8-8d5d-6287f9ee50ad)



# What is Azure DevOps?

Azure DevOps is a suite of tools that helps teams to plan, develop, test, and deploy software. It provides a single place for teams to collaborate on code, track work, and manage releases. 
![image](https://github.com/lephuocduc/TheLabGuide/assets/37317309/e808db0b-5815-4b21-a3a7-b8e074543bd0)

This consists of the following services:
1. **Azure Repos:**  is a code repository service that helps you to manage your code in a secure and scalable way.

3. **Azure Pipelines:** is a continuous integration and continuous delivery (CI/CD) service that helps you to automate the process of building, testing, and deploying your code.

4. **Azure Boards:** is a work management service that helps you to track the progress of your projects.

5. **Azure Test Plans:** is a test management service that helps you to plan, execute, and manage your tests.

6. **Azure Artifacts:** is a package management service that helps you to manage your dependencies and artifacts.

So in summary it's a one-stop shop that makes implementing DevOps processes much easier for developers, but also allows them to plug in any other preferred third-party tools and services they may already be using in place of the included tools if they so wish.

## In this lab, you will:
1. Create a Kubernetes cluster in Azure using the Azure Kubernetes Service (AKS)
2. Create a project in Azure DevOps
3. Set up a Continuous Integration and Continuous Delivery pipeline in Azure DevOps to deploy a demo website to AKS
4. Pull the demo website code locally and make some changes
5. Use your new pipeline to push these changes directly to your demo website in AKS and view the results

Once we're done, you'll end up with an architecture looking similar to this (except you're the engineer!):
![image](https://github.com/lephuocduc/TheLabGuide/assets/37317309/aa416ab5-b788-49c5-acc3-3be524c3ffee)

# Let's get started

## Create a Resource Group
Resource groups in Azure can be likened to logical buckets where you can organize your resources. In this lab, we will utilize the Azure CLI and Azure Cloud Shell to create and manage our resources.

1. Navigate to the Azure Portal and click on the Cloud Shell icon (>_ on the top right panel of the portal) -> Create Storage and select 'Bash (Linux)'

![image](https://github.com/lephuocduc/TheLabGuide/assets/37317309/d1a3bd4c-89f1-4a83-9000-6b4b6c5ea6b2)
![image](https://github.com/lephuocduc/TheLabGuide/assets/37317309/87cb0032-feda-437a-8098-153427a5e756)
  
2. Create a resource group and specify your preferred **region**.  Some examples are eastus, westeurope, westus.  Whichever you pick, please use that region for the rest of the lab when asked to create a new resource.

``` bash
az group create --name <RG name> --location <location>
```
``` bash
az group create --name DucLe-RG1 --location westus
```
![image](https://github.com/lephuocduc/TheLabGuide/assets/37317309/5bb160e2-7d1c-45de-8c9e-46dbf1f2fda6)

## Deploy Azure Kubernetes Service (AKS)
To create your cluster, copy and paste the below into your cloud shell.  Choose a unique name for your AKS cluster.
> NOTE: AKS cluster names must contain only letters, numbers and hyphens, and be between 3 and 31 characters long.

``` bash
az aks create --resource-group <RG name> --name <cluster name> --enable-addons monitoring --kubernetes-version <version number> --generate-ssh-keys --location <location>
```
``` bash
az aks create --resource-group DucLe-RG1 --name ducle-aks --enable-addons monitoring --kubernetes-version 1.26.3 --generate-ssh-keys --location westus
```
The AKS cluster will take a little while to deploy.  Now is the ideal time to get yourself a drink and give your eyes a 5 minute screen break!
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/993c6226-2897-4bc2-a056-d79572c72f6a)

Get credentials to access the cluster using the az aks get-credentials command.
``` bash
az aks get-credentials --resource-group DucLe-RG1 --name ducle-aks
```
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/57cc8826-e738-4a35-ad2b-673c1e3a0da6)


## Deploy Azure Container Registry (ACR)

When the cluster is deployed, we can move on to creating our container registry. As mentioned in the glossary, we can use ACR to securely host our application container images. Copy and paste the belowand give your ACR a unique name **between 5 and 50 characters, letters and numbers only**

``` bash
az acr create --resource-group <RG name> --name <ACR instance name> --sku Standard --location <location>
```
``` bash
az acr create --resource-group DucLe-RG1 --name ducleacrinstance --sku Standard --location westus
```
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/4e81303b-7b10-4f13-9a35-887c4ebb9e26)

To access other Azure Active Directory (Azure AD) resources, an AKS cluster requires either an Azure Active Directory (AD) service principal or a managed identity. When you created the AKS cluster, a Managed Identity was automatically generated.  We need this to authorize the AKS cluster to connect to our Container Registry and pull down container images.

This identity can be used to authenticate and authorize the resource to access other Azure resources, such as Azure Key Vault, Azure Storage, Azure SQL Database, or Azure Container Registry. In our scenario, we will need to access a container registry - both to push and pull images to get our website running on a Kubernetes cluster. The steps below show you how to get the ID of the Managed Identity for AKS and the resource id of our container registry.  With these, we can create a role assignment for the Managed Identity, **acrpull**, which allows for pulling of images by the Managed Identity. 

Pop the below into a text editor and then make sure you replace **aks_cluster_name** with whatever your named your AKS cluster, and replace **acr_name** with whatever you named your ACR. Then go ahead and paste it into the terminal.


``` bash
# Get the id of the managed identity configured for AKS
CLIENT_ID=$(az aks show --resource-group DucLe-RG1 --name ducle-aks --query "identityProfile.kubeletidentity.objectId" --output tsv)

# Get the ACR registry resource id
ACR_ID=$(az acr show --name ducleacrinstance --resource-group DucLe-RG1 --query "id" --output tsv)

# Create role assignment
az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/f90af84a-ded6-46ae-9d73-20ed1aad457e)




