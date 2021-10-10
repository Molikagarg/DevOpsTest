# DevOpsTest
Q1- SCENARIO
Car rental company -- FastCarz, .net Web Application and Web API &  Azure cloud using Azure Web App Service
and Web API Service.  code repository -- Azure GIT.

I am using the YAML pipeline to setup the Azure DevOps Pipeline service for all the requirements stated- 
<1>The build should trigger as soon as anyone in the dev team checks in code to master branch
Trigger is indicated by the Trigger keyword in the pipeline. Trigger will include the branches on which build triggers automatically as the code is checked-in.
e.g. trigger:
        branches:
          include:
            refs/heads/master
            
<2> There will be test projects which will create and maintained in the solution along the Web and API.
The trigger should build all the 3 projects - Web, API and test. The build should not be successful if any test fails.
steps:
    - task: DotNetCoreCLI@2
      inputs:
        command: 'restore'
        projects: '**/*.sln'
        feedsToUse: 'select'
        vstsFeed: '<your-feed here>'
    - task: DotNetCoreCLI@2
      inputs:
        command: 'build'
        projects: '**/*.sln'
    - task: DotNetCoreCLI@2
      inputs:
        command: 'build'
        projects: 'src/SimpleApi/SimpleApi.csproj'
    - task: DotNetCoreCLI@2
      enabled: true
      displayName: Run Unit tests
      continueOnError: false  // Build should not be successful if any test fails.
      inputs:
        command: 'test'
        projects: '**/*Tests/*.csproj'
        testRunTitle: 'Run Tests'
        
<3> The deployment of code and Artifacts should be automated to Dev Environment. 
        This can be achieved by configuring the enviornments in the Azure DevOps Environments under Azure Pipelines section.
        then this environment will be used in yaml pipeline with the Environment keyword for the deployment in the particular environment.
        it is shown in the Pipeline. devopsTest.yml file as STAGE: Dev
        
<4> Upon successful deployment to Dev Environment, deployment is promoted to QA & PROD through automated process- This is also shown in the devopsTest.yml pipeline code as Stage: QA & Stage: Prod respectively.
        
<5> For each of these QA & Prod environment, configured in the Azure DevOps Environments under Azure Pipelines section, we have set the Approvals under the Approvals section in the environment setting. This will allow the code deployment to the particular stage only after the approvals from the required approvers confgured. 

        
Q2- SCENARIO
<2> Tools to create and store the terraform templates
        1. Azure subscription.
        2. Choose the desired subscription to store cloud shell resources in and select create storage.
        3. Create the Terraform Config file. Terraform file is written in HCL - Hashicorp configuration language.
        4. Terraform code is stored in configuration files with *.tf extension.
        5. Create main.tf file to deploy the resource group to azure subscription.
        6. Init -- terraform init -- initialize the config file.
        7. terraform plan-- to see the plan in the output. Each affected resources will be listed in the output.
        8. terraform apply 
        
<3> Steps to create the automated deployment
        1. Maintain the deployment code as source in Azure Repos.
        2. Create the service connection for conncetion between the Azure Devops and Azure portal for resources deployment in cloud.
        3. Develop the separate Deployment pipeline for Azure resource deployment. 
        4. This pipeline will use the Azure Resource deployment task or we can also use the Azure CLI task etc. for deployment of azure resources which will ask for the Azure Subscription (i.e. service connection created above), Template file (path where it is stored), Parameters file etc.
<4> 
<5> To access the passowrd stored in the Azure Keyvault, 
        secrets can be kept in Azure Pipelines - Library -- refering to azure keyvault -- with access to only admin personel  
        To access the keyvault secrets in Terraform template - use the Azure Keyvault resource -- pointing to the keyvault in azure, refer to the secret in keyvault by using the reference instead of vaule. 
        This way secrets will be safe also.
        
