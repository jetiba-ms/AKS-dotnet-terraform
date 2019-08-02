A - On Azure Devops:
1. Create 2 new repo in the Azure DevOps project, one for each application we will deploy on the cluster, name them **messagesenderapi** and **messageprocessor**. As a best practice, we keep the 2 application detached, because each of them could follow different roadmap, and have different time for commits and releases. So the life cycle pf each application could be completely different. 
2. Clone the code of the 2 applications in each of the repo created in the previous step
3. In _Project Settings_ -> _Service Connections_ -> _+ New Service Connection_, select **Docker Registry** to create connection for the Azure Container Registry (this is created with the IaC pipeline <add link>)
4. In **Pipelines** section of Azure DevOps portal, click _+ New_ -> _New build pipeline_ -> _Azure Repos Git (YAML)_, select the **messageprocessor** repo and in _Configure your pipeline_ page, select _Existing Azure Pipeline YAML file_ and select the `/azure-pipelines.yml` file in the drop down menu
5. Before to proceed with the run of the build, you have to check the file and setup variables (from outputs resulted in the point B.6 of the **Prerequirements and Infrastructure deployment** chapter, as per the following list:
- AKS_CLUSTER_NAME
- AKS_CLUSTER_RG
- ACR_NAME
- EH_NS_NAME
- STG_NAME
- STG_CONTAINER_NAME
6. Save and run the pipeline
7. Do again steps 4 for **messagesenderapi** and in the step 5 add all the variables, except STG_NAME and STG_CONTAINER_NAME

Note that, unfortunately, at the moment it isn't possible to automatically take the outputs of a pipeline in one repo to a pipeline in another repo. It is not the best, but it is important to keep applications repo and pipelines separated from the infrastructure repo and pipeline, due to the fact that they have different update and life cycles.

B - On your development machine:
1. Check the correct running of the applications in the AKS cluster with the command `kubectl get pods -n arch-aks-sb-dotnet` to see the pods created and if they are in execution
![cmd-pod-img.jpg](/.attachments/cmd-pod-img-33fc4901-c007-4393-91dc-60d504c25b77.jpg)
2. `kubectl get service -n arch-aks-sb-dotnet` to see the external IP of the msgsenderapi services to send new messages to the event hub and check if everything is working well
![cmd-ip-img.jpg](/.attachments/cmd-ip-img-d868916d-7e21-4d38-9194-118ac5761354.jpg)
3. Copy the IP, open Postman and send an HTTP POST to `http://<IP address>/api/value`s with as a body something like `{"values": "my-message-here"}`, if it is working well it returns **200 OK** response
4. In the command prompt run `kubectl logs -f <pod-name> -n arch-aks-sb-dotnet` and see the message processed 
![msgproc-img.jpg](/.attachments/msgproc-img-65471a5d-ac67-44cc-ad8c-06e254e7ded9.jpg)
