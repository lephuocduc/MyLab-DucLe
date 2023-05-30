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
2. 
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

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/70b6f52f-0055-4f74-95d2-113a9df96201)


After a few minutes, the release should be successful. If you get any errors regarding your container registry in the AKS deployment phase, go back to your release definition and confirm that your Azure subscription and container registry are selected - then save the definition and repeat the steps above.

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/757ec605-7b4d-4cad-a524-cfedc1edc4ac)


## View your newly deployed website

Now - Azure DevOps has deployed the website to a Kubernetes cluster - but how can we see it?

1. Go back to the Azure Portal and open Cloud Shell once again. We need to tell the Shell which Kubernetes cluster we want to work with, and we do this by getting the context:
``` bash
az aks get-credentials -g DucLe-RG1 -n ducle-aks
```

kubectl is a command line tool for working with our Kubernetes service.  Now, let's check if our Pods are up and running.  If so, we should see both the front and back end Pods up and running:
``` bash
kubectl get pods
```
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/852d5d4c-20ae-4bb5-9eef-1ad717debaa7)

Now we need to find out the public IP that the website is deployed to.  Kubernetes supports exposing our application via two methods: Load Balancer and NodePorts.  In this exercise, we can look at our mhc-aks.yaml file to confirm that we are using the Load Balancer service in Kubernetes to expose our application front end to a public IP:

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/2dc3b30a-a8ad-4eaf-9700-4a2644b23d1b)

To find out what public IP address has been assigned, we can type the following into our command prompt window:

``` bash
kubectl get service mhc-front
```
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/090cf207-f5cf-4d3a-80ae-2a6dafb16b2c)

Copy and paste the external IP into a browser window.  You should see your newly deployed Health Clinic application.

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/ca7f24fd-dc17-4e98-aa6b-ca911c462564)

## Set up Continuous Integration

Now for our final step, we want to complete the DevOps pipeline that we've set up. Right now, we have successfully set up 'Continuous Deployment', which essentially means when we trigger a Build, like we did in a previous step, the application's code is bundled up, then sent over to the Azure Kubernetes Service, and deployed into the wild, all without us having to perform any steps manually.

But what if we want all of this to happen as soon as we make a change to the application's code? Well, this is what we call Continuous Integration. This is what enables developers to publish changes to application code much more quickly, and respond to their user's needs in a much more agile way.

Let's set this up. Firstly, go back to Azure DevOps and open up the **Build and Release** page, then click your build and navigate to the **Triggers** tab. We want to check the box that says Enable continuous integration. Then click 'Save'.

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/a31c77ee-0427-4028-9b56-1b1b3c842f11)

This will now trigger our Build automatically when a change has been made to the application's code on the *master* branch.

> Branches allow developers to work on a new feature in a separate branch from the main code, then re-introduce their code when it's ready, by performing something called a *merge*. We'll just be using the master branch in this lab for simplicity.

Let's test if this works. Head over to the Code page which should open your code files. On the top right, you'll see an option called 'Clone' - give it a click and copy the Clone URL to your clipboard.

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/285b1c58-b200-4ee7-b45a-80f1047c2ad3)

Think of this as a direct link to your code, which we'll use to download it to your machine. We could make quick changes to the code in Azure DevOps itself like we did before; however to simulate how developer's typically work with code locally then push up to a remote master, we'll download it to our machine to work on it. Copying code in this way is called a `Git Clone` operation.

## Updating the Code

One really cool feature of Cloud Shell, is that it comes with Git tools installed! If you already use a code editor, like Visual Studio Code, feel free to use that in the following steps. You also need to make sure you have Git installed on your local machine.

> Git is a framework that is widely used by developers to track code changes and collaborate across various people and teams on a single source code. Imagine the complexity of working on 20 Word documents seperately and then trying to merge them together - this is what Git helps us with in the code world!

First, we need to tell Git who we are so that it can track our changes. Open a Cloud Shell session and enter the following, replacing NAME and EMAIL with a nickname and the email you used when creating your Azure DevOps account:

```
git config --global user.name "NAME"
git config --global user.email "EMAIL"
```
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/7633c619-7b85-4e7a-9dfa-c41a3600ecc0)

In order to get the code from our repo, we will need to authenticate. There are a few ways we can do this, and Personal Access Tokens are preferred and the most secure method. You can read about them [here.](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops)

In Azure DevOps, click on your profile icon (top right) and then select Personal Access Tokens.

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/eb0b12c1-8933-48ef-9a6e-0ea2af8510e5)

Click on '+ New Token' and fill in the details. Ensure you give your PAT a name, and follow the below checklist before hitting Create.

- a descriptive name
- select 'custom defined'
- assign it 'read and write' permissions under Code

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/b54a903d-9e2f-4cd0-ad33-93dcfd491a6d)

After creation, you will see the message below - make sure you copy your PAT and paste it into your notepad - you won't be able to retrieve it. 

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/ca2096e7-2495-4088-80b3-f6bf84118395)

We have just created an access token that can be used to perform Code operations in our Azure DevOps organisation. 

Now we'll use Git to grab the code from our remote repository in Azure DevOps. Type the following, replacing LINK with the Clone link you copied from Azure DevOps.

```
git clone LINK
```
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/3ad5a23b-4911-46eb-894e-7f0a197dde1e)

You will be prompted for a password. Copy the PAT you just created, paste it into Cloud Shell, and hit enter. You should see something like the below:

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/b8158979-a864-4bc0-9aaf-534ff2f6f44f)

Great, now type:

```
ls
```

You should see a folder called 'AKS'. This contains all of the websites code and is identical to the code in our Azure DevOps repo. Now let's edit it. Change directory by typing:

```
cd AKS
```

Click on the curly braces (they look like this: { } ) in the Cloud Shell to open the built in code editor. 

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/433f7f92-b0b5-47de-83df-d68a4f48938f)

Once it has launched, you can resize the window to make it easier to work with. 

Now, in the Explorer pane on the left, navigate into the folder containing our website's Home page by following this path: `src > MyHealth.Web > Views > Home` then opening `Index.cshtml`.

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/893ea330-c005-4f15-9106-fc02ee624ed8)

This is a C# HTML page and essentially renders the structure and content of our medical website's homepage. Let's make a little change to the page just so we can see our changes automatically being pulled through to our live website running on Azure Kubernetes. Change any of the page text (careful not to change anything inside a html tag - < > ), for example I've modified 'Connect with your health' to 'I've just changed this page'. Then hit 'Save' (or `Ctrl + S`).

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/ee9ab075-58ec-469d-abcb-54c95fcb4872)

Now that we've modified the code, we need to tell Git that we'd like to commit the change and then send it up to the remote Azure DevOps repository to be merged into the master branch.

We can do this directly in Cloud Shell. First, let's check if Git has picked up our change.

```
git status
```
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/bd337f0d-0ef6-4b7a-a4a0-0e42b30b3f0a)

Git identified the modified file. Now let's commit the change.

```
git commit -a -m "changed Index.cshtml"
```
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/7edc086b-42a7-4b30-85d8-a2eeb436d28f)

This has now updated our **local** master branch with the changes we made, but we haven't yet told Azure DevOps about this. Git keeps a local record of all your code changes / commits, and another on the server (Azure DevOps), so we need to now synchronise the two. Type the following git command:

```
git push
```
You may be prompted for a password; enter the same PAT you used earlier.

Now Git will first pull down any changes that have been made remotely since we copied it to our machine, it will check if there are any conflicts, and if not (and there shouldn't be since no-one else is using our repo!) it will then merge and push the code back up to Azure DevOps.

Let's double check that this has happened. Go back to Azure DevOps in your web browser and refresh the Code page. You should see your commit message has appeared next to the 'src' folder (since this contains the homepage code you changed).

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/956e126e-61b7-4a59-b13c-67c25d2f09b9)

That's not all. Because we set up the Continuous Integration trigger, we should now no longer have to manually click 'Queue new Build' to push code to our live website. Let's confirm this by heading to the 'Build and Release' page.

You should see that a new Build is already in progress, along with the latest commit message we pushed.

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/a59c22eb-8d22-4e64-8205-57473141e5c3)

If all goes well, the build should then initiate a Release like before, thanks to our Continuous Deployment pipeline, which will make its way to your Azure Kubernetes Service and out on the wild web. Nice one!

Back to the website, we can see the title has changed successfully!
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/09e3dd28-9cc2-4d39-bb02-a4709be7a759)

# So what have I just achieved?

Congratulations on completing the lab, we hope you found it useful and engaging. To summarise, you have:

1. Created a Kubernetes cluster in Azure and a project in Azure DevOps to host and run your code
2. Set up a Continuous Delivery pipeline in Azre DevOps & linked to your Azure environment (no small feat!)
3. Tested the pipeline and monitored your application running in the Kubernetes service
4. Set up a Continuous Integration pipeline to feed new code changes straight through to the CD pipeline
5. Used Git and VS Code to Clone the code repository to your machine, then edited the code and pushed it back to Azure DevOps
6. Automatically triggered a new build through CI which has pushed your changes straight to your live website

DevOps is by no means simple, but you've covered a lot of ground and tackled the core principals of CI, CD and working with code. Well done.
