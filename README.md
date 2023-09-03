# CDK Pipelines in azure DevOps

This repository contains de CDK pipelines definitions for integrating azure DevOps and AWS, based on simple authentication and multi account setup in AWS.

# Architecture Diagram

![Diagram architecture](img/DevSecOps-DevSecOps-Serveless-at-Scale-%20AzureDev.jpg)

# Requirements

* Bootstrap accounts for CDK deployments.
* A Service Connection with right permissions.


# How to

1. Create a variable group with values for environments, for example:

```bash

az pipelines variable-group create --name cdk_pipelines_delivery --variables dev_account=123456789012 dev_region=us-east-2 --authorize true --description "Group for lab Pipelines Delivery" --project Delivery

```


2. Create an azure pipeline file into cdk project to use these templates, for example:

```yaml

# File: azure-pipelines-c#.yml
variables: 
- group: 'ephemeral_envrionments_demo'


resources:
  repositories:
    - repository: cdk_pipelines
      type: git
      name: DevSecOps/cdk_pipelines
trigger:
- master

pool:
  vmImage: ubuntu-latest


stages:
- template: templates/ci_cd.yaml@cdk_pipelines  # Template reference

  parameters:
    Project: ephemeral_env_core
    Language: typescript
    Action: deploy
    ServiceConnection: EphemeralEnvDeployment
```

Learn how in: 
- [Azure Devops and CDK- 1](https://dev.to/aws-builders/devsecops-with-aws-integrate-azure-devops-for-cdk-deployments-part-1-c4e) 
- [Azure Devops and CDK- 2](https://dev.to/aws-builders/devsecops-with-aws-integrate-azure-devops-for-cdk-deployments-part-2-33i5) 
