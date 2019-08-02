A - On Azure Devops:
1. Create 2 new repo in the Azure DevOps project, one for each application we will deploy on the cluster. As a best practice, we keep the 2 application detached, because each of them could follow different roadmap, and have different time for commits and releases. So the life cycle pf each application could be completely different. 
2. Clone the code of the 2 applications in each of the repo created in the previous step
3. In _Project Settings_ -> _Service Connections_ -> _+ New Service Connection_, select **Docker Registry** to create connection for the Azure Container Registry (this is created with the IaC pipeline <add link>)
4. in **Pipelines** section of Azure DevOps portal, click _+ New_ -> _New build pipeline_ -> _Azure Repos Git (YAML)_, select the repo of the application that you want to deploy and in _Configure your pipeline_ page, select _Existing Azure Pipeline YAML file_ and select the `/azure-pipelines.yml` file in the drop down menu
5. Before to proceed with the run of the build, you have to check the file and setup variables (from outputs resulted in the point B.6 of the **Prerequirements and Infrastructure deployment** chapter, as per the following list:
- AKS_CLUSTER_NAME
- AKS_CLUSTER_RG
- ACR_NAME
- EH_NS_NAME
- STG_NAME
- STG_CONTAINER_NAME
Note that, unfortunately, at the moment it isn't possible to automatically take the outputs of a pipeline in one repo to a pipeline in another repo. It is not the best, but it is important to keep applications repo and pipelines separated from the infrastructure repo and pipeline, due to the fact that they have different update and life cycles.