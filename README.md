# DevOpsTest
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
        
