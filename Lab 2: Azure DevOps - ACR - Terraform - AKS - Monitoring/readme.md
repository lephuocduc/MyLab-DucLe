# What you will learn

In this tutorial/lab, you will learn:
- Initial setup of Azure DevOps to begin deploying to Azure using Pipelines as code
- Deploy Azure resources using Terraform modules
- Deploy a test application to Azure Kubernetes Service 
- An understanding of CI/CD with automated application deployments
- Test your deployed Azure resources using automated testing
- Reviewing monitoring and alerting using Application & Container Insights

This setup is based on a somewhat "real-life" scenario and setup mirrors an example of a real-world setup!

# Tutorial/labs format


#### 1. Initial Setup starts you off with setting up:
  - ##### Azure DevOps Setup:
    - Azure DevOps Organisation Setup
    - Azure DevOps Project Creation
    - Azure Service Principal Creation
 
  - ##### Azure Terraform setup
    - Create Blob Storage location for Terraform State file
  
  - ##### Create Azure AD Group for AKS Admins
    - Create Azure AD AKS Admin Group


#### 2. Setup Azure DevOps Pipeline The purpose of this lab is to create all of the Azure cloud services you'll need from an environment/infrastructure perspective to run the test application.
  - ##### Pipeline setup
    - Setup Azure DevOps Pipeline


#### 3. Deploy Application to Azure Container Registry Deploy sample Application to Container Registry.
  - ##### Deploy Application to Azure Container Registry
    - Build the Docker Image Locally
    - Run The Docker Image Locally
    - Deploy sample Application to Container Registry


#### 4. Deploy Application to Azure Kubernetes Cluster
  - ##### Add AKS ACR Role assignment
    - Terraform to add role assignment for AKS managed identity to access the deployed ACR

  - ##### Add Application Insights to Terraform
    - Application Insights will be used to monitor the application once deployed!

  - ##### Add Azure Key Vault to Terraform
    - Azure Key Vault will be used to store secrets used within your Azure DevOps Variable Group.

  - ##### Update Pipeline to Deploy Application to AKS
    - Update Pipeline to Deploy asp Application to AKS


#### 5. Introduce CI/CD
  - ##### Introducing CI/CD to your pipeline
    - Begin CI/CD with Pipeline Trigger for automatic pipeline runs

  - ##### Automated deployment of your AKS Application
    - In previous labs; the application was initially manually setup for its build tag. In CI/CD, this would be automated and the Application on the AKS cluster would update each time the pipeline has been ran.


#### 6. Testing your deployed Azure Infrastructure
  - ##### Testing Infrastructure using Inspec
    - Using Inspec-Azure to test your Azure Resources

  - ##### Inspec Testing using Azure DevOps Pipeline
    - Run Inspec-Tests using Azure DevOps
    - View Inspec reports in Azure DevOps


#### 7. Monitoring and Alerting
  - ##### Azure Application Insights
    - Using Application Insights to view telemetry data!

  - ##### Azure Application Insights Availability Tests
    - Configure availability test using Application Insights

  - ##### Log Analytics Container Insights
    - Reviewing Log Analytics Container Insights

to be continued...
