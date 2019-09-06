# AKS-dotnet-terraform

This repo contains the step-by-step guide files for an Azure DevOps, Terraform and Azure Kubernetes Service tutorial.
You can learn how to define and prepare a CI/CD process for the creation of the Kubernetes cluster on Azure and the deploy of the applications.

To do that you can follow the guide in this way:
1. [Prerequirements and Infrastucture deployment](https://github.com/jetiba-ms/AKS-dotnet-terraform/blob/wikiMaster/Prerequirements-and-Infrastructure-deployment.md)
2. [Applications deployments via CI/CD on Azure DevOps](https://github.com/jetiba-ms/AKS-dotnet-terraform/blob/wikiMaster/Deploy-applications-on-the-AKS-cluster.md)

There are other 3 repos:
- Terraform scripts define the infrastructure on Azure **[iac](https://github.com/jetiba-ms/AKS-dotnet-terraform-iac)**
- A sample application to process messages **[msgprocessor](https://github.com/jetiba-ms/AKS-dotnet-terraform-msgprocessor)**
- A sample API that sends messages to an Azure Event Hub **[msgsenderapi](https://github.com/jetiba-ms/AKS-dotnet-terraform-msgsenderapi)**

This tutorial is though as a quickstart guideline for concepts like **Infrastructure-as-Code** and **CI/CD with Kubernetes**.

It is still a work-in-progress project, some topics that will come are:
- cluster monitoring
- identity management in the cluster
- cluster autoscaler
- KEDA, Kubernetes Event-Driven Autoscaling
- using Virtual Node with Azure Container Instances
