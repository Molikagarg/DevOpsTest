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
