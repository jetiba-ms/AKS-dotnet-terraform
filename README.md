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

If you want you can also execute the steps for a manual configuration, following the file marked as [Manual], in this order:

0. [Configure and install all required tools on the development machine](https://github.com/jetiba-ms/AKS-dotnet-terraform/blob/wikiMaster/%5BManual%5D%20Development%20machine%20configuration.md)
1. [Create AKS cluster](https://github.com/jetiba-ms/AKS-dotnet-terraform/blob/wikiMaster/%5BManual%5D%20Create%20AKS%20cluster.md)
2. [Registry setup](https://github.com/jetiba-ms/AKS-dotnet-terraform/blob/wikiMaster/%5BManual%5D%20Registry%20Setup.md)
3. [Configure OSBA](https://github.com/jetiba-ms/AKS-dotnet-terraform/blob/wikiMaster/%5BManual%5D%20Configure%20OSBA.md)
4. [Deploy images on AKS using Helm](https://github.com/jetiba-ms/AKS-dotnet-terraform/blob/wikiMaster/%5BManual%5D%20Deploy%20images%20on%20AKS%20using%20Helm.md)

It is still a work-in-progress project, some topics that will come are:
- cluster monitoring
- identity management in the cluster
- cluster autoscaler
- KEDA, Kubernetes Event-Driven Autoscaling
- using Virtual Node with Azure Container Instances
