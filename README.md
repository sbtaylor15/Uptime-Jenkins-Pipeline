# Summary
The Uptime-Application cosolidates multiple components that are built by Jenkins into a single application package.  Each of the components have thier own Jenkins CI process for creating, publishing artifacts and deploying to Development servers using DeployHub.

## History
The [Wiki](https://github.com/OpenMake-Software/Uptime-Application/wiki) will contain a full history (builds, development deployments and the Continous Deployent Pipeline to Production).

## Approving Development Change for the Continous Deployent Pipeline
A development team lead will approve the appropriate application version to proceed down the Continuous Deployment Pipeline when they are statisfied with the integration and testing that they have performed. 

The Application Verison Wiki page needs to be updated with an "Approve" message to perform the approval.  Once approved a Jenkins Pipeline will be initiated to carry that application version through the stages of the pipeline.  These stages typically include Integration Deployment and Testing, QA Deployment and Test, and finally Production Deployment.

The Pipeline will only proceed to the next stage if sucessfull.
