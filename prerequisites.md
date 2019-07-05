1. Create an Azure Service Principal `az ad sp create-for-rbac -n mySPforAKSPipeline`
1. Create **Service Connections** in Azure DevOps Project Settings
- **Azure Resource Manager** with the details of SP
- **Docker Registry** for the ACR (created with the IaC pipeline <add link>)
