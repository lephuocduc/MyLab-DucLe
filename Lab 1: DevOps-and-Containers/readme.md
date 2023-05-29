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

## Grant permission for Managed Identity to perform actions on ACR
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

## Deploy an Azure SQL Database

We need to deploy a database for the back end of our application. Azure SQL Database is a general purpose relational database managed service. There are a number of deployment options, and for this lab, we will deploy a single database managed via a SQL DB Server. Give your SQL Server a unique name (**unique-sqlserver-name** below)

> NOTE: SQL Server names must be **lowercase** and unique. This is very important - otherwise your lab won't work.
``` bash
az sql server create -l westus -g DucLe-RG1 -n ducle-sqlserver -u adminuser111 -p Adminuser123$
```

Once that is successful (it may take a few minutes), create the database, making sure to use the server name you chose above, and keep the database name **mhcdb**. 
``` bash
az sql db create -g DucLe-RG1 -s ducle-sqlserver -n ducle-sqldb --service-objective S0
```

Go the resource group 'DucLe-RG1'. Verify that you see the resources below (with whatever you named them). 
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/69c725d7-6698-402c-bdda-e8eb54ca3651)

We need to make a note of some of the resource names.  We will use these when creating our CI/CD pipeline in Azure DevOps.  Make sure you note down:

* Your AKS cluster name

* Your Container registry Login server name
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/02c497b7-4e1b-49b8-b4c7-9227993fed36)

* Your SQL Server name
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/a4f7dac1-c7a8-4e58-8374-3fcffcf10b3e)


## Create an Azure DevOps account and generate a demo project

Now we will generate our demo project, using Azure DevOps Generator!

Go to [Azure DevOps Generator!](https://azuredevopsdemogenerator.azurewebsites.net) (right-click and open in a new tab) and sign in with your Azure DevOps account.  If you don't have one, click 'Get started for free' and follow the instructions.
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/1e5f368f-80a4-4b30-b0b4-845bc8781a51)

You're all setup with an Azure DevOps account now! Go back to the demo generator and sign in. Accept the terms and conditions and proceed to choosing a project. Select your account name, and choose the project specified in the image below (click the DevOps Labs and then the AKS project) Call it Lab1.
Install the extension on your Azure DevOps account.  Once installed, return to the demo generator and create your project.  To know more about [how to install free extensions for Azure DevOps!] here(https://docs.microsoft.com/en-us/azure/devops/marketplace/install-vsts-extension?view=vsts)
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/789d8ea7-4c28-40e5-8b77-5aae7f41b55e)

After a minute or two, your project will be successfully created. Navigate to your project - it's time to start building our build and release pipeline!

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/75eb70dd-936f-4ee3-b5a0-3aed072791a2)


##  Explore repository

Now, we will explore our project code.  Select Repos and then Files on the left hand side menu:
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/d2b5ca71-5d13-4d06-942e-08cd77584844)
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/ebd2fed9-cde0-4966-bd97-3d29a0f9c482)

Our repository contains the code for a .NET Core MVC (Model View Controller) website (in folder 'src').  We have some other files in this, and in the root of our project, that enable us to deploy the website to containers:


**dockerfile** - This file enables Docker to build an image automatically by reading the instructions contained within.  In this case we will be pulling the aspnetcore:1.0 image from the Microsoft Docker hub.

**docker-compose.yml** - This file defines the image that will be used and points to the Dockerfile above which we used to build the image for us.

**mhc-aks.yaml** - This is our Kubernetes manifest file.  In here, we define the deployments, services and pods that we need for our application to run.


## Build Definition

Now we can edit our build to correctly build our Docker image.  Select our build definition 'MyHealth.AKS.build' and click the edit button.
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/148d95f0-4a92-4359-a47a-dc383beb5cca)

You will see two 'Replace Tokens' tasks and four Docker Compose tasks.  The replace tokens task is a neat little feature that will replace some of the hard coded values in our code sitting in source control.  This is especially valuable when deploying the same code to different environments, which will have different databases, container registries, AKS clusters etc. 
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/1bba7573-71dd-4357-a385-2f3187022807)

1. Select 'Variables' from the bar at the top, and then update each of the variables below to match the values you made a note of earlier:

- ACR Name
- SQLpassword
- SQLserver
- SQLuser
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/4e47677e-c3fe-4117-84bd-5c65dc48bb2f)

You will need to repeat the next step for each Docker build task highlighted below:


2. Under 'Azure Subscription' select the your subscription.  The first time you do this, you will need to Authorize the service connection (this step allows you to deploy from Azure DevOps into your Azure subscription).

1. Under Azure Container Registry, select the container registry you created earlier.
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/2c940b51-05c9-4b3c-be9c-b22eb5d8e72c)

Repeat for Build, Push and Lock tasks.  Save the build, but do not queue anything just yet.


## Release Definition

Navigate to Releases on the left hand menu, click the ellipsis next to MyHealth.AKS.Release and click 'Edit':
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/4b8722d5-9a5a-400b-98ac-8ef7152040f0)

You will see our release pipeline.  
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/2c486668-f1ae-422d-b1db-69b85f994879)

Once a new build is ready, we have a release ready to deploy automatically.  The first thing we need to do is update some of our variables.  Click 'Variables' just above your pipeline.
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/db46e6d9-cb13-4751-874b-82da688046de)

Update each of the variables below to match the values you made a note of earlier:

- ACR Name
- SQLserver
- SQLuser

Now that our variables are referencing our Azure resources, we can edit the Release tasks.  Click the Tasks menu item (it should have a red exclamation mark beside it) and click 'Dev'.

In the 'Execute Azure SQL: DacpacTask', update the Azure Subscription to the one you authorized earlier.
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/f60c946f-a476-4827-8114-bad56d35b86f)

Under the AKS Deployment phase, click the first task **Create Deployments & Services in AKS**. Choose uour subscription, resource group and Kubernetes cluster in the dropdown boxes, then scroll down to 'Secrets'.

 Again, choose your Azure subscription from the drop down box.  Next, choose your Container Registry from the drop down box.  A secret called mysecretkey is created in AKS cluster through Azure DevOps by using a command 'kubectl create secret' in the background (we will use more kubectl later in the lab). This secret will be used for authorization while pulling myhealth.web image from the Azure Container Registry.
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/19defda7-0d76-42e1-8c92-374812a4e254)

Update the Azure Subscription, Resource Group and Kubernetes cluster from the dropdown. Expand the Secrets section and update the parameters for Azure subscription and Azure container registry from the dropdown.
Repeat similar steps for Update image in AKS task.
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/fa14be20-205d-431c-b4ec-9e938b7d5cdb)


We can now move on to the second task in our AKS deployment phase.  Simply repeat the steps above and save your release.


## Kick off our build and release pipeline

We are ready to deploy!

Go back to the build definition you edited earlier.  Click 'Queue':
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/371f748f-b4be-4777-b9a8-ab417235828d)

Accept the defaults and run it:

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/d4022cde-ef60-4f59-bfdd-9887c5f22ca8)

You can view progress by clicking on it:
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/f226c2e0-6120-4242-8a58-2a9233a3fc86)

You can view detailed logs by clicking on any of the steps in the process.  The build should succeed - if so, a release will automatically be kicked off as we have enabled continuous delivery.  Let's check it out.
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/83633a23-52cc-40b1-934f-eaa892476fdf)


Navigate to Release and select the new release. You may have to wait for a minute or so before it appears. You'll see something like the below when you click on it.


![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/abf8c25a-8152-4104-bd4b-7dccedc7e038)
Here we can see that the our successful build has triggered a new release into our Dev environment. Under the Dev environment, click 'In progress' to see detailed logs of what's happening.

After a few minutes, the release should be successful. If you get any errors regarding your container registry in the AKS deployment phase, go back to your release definition and confirm that your Azure subscription and container registry are selected - then save the definition and repeat the steps above.



to be continued...
