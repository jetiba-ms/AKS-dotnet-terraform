0 - First of all, clone or download the code from the following link <**_TODO_**: add link>

A - On Azure Subscription:
1. Create an Azure Service Principal `az ad sp create-for-rbac -n <sp_name>`, it returns the follow result, store it:

```
{
  "appId": "xxxxx-xxx-xxx-xxx-xxxxx",
  "displayName": "<sp_name>",
  "name": "http://<sp_name>",
  "password": "xxxxx-xxx-xxx-xxx-xxxxx",
  "tenant": "xxxxx-xxx-xxx-xxx-xxxxx"
}
```

2. Create an Azure Storage Account to keep the Terraform status, store the storage name 
3. Create a Blob Container, name it `state`

B - On Azure DevOps:
1. Create a new project
2. Crate a new repo, name it `iac`
3. Create **Service Connections** in Azure DevOps Project Settings: in _Project Settings_ -> _Service Connections_ -> _+ New Service Connection_, select **Azure Resource Manager** and fill the form with the details of Service Principal created at A.1.
4. Go to the folder where you clone/unzip the project at the point 0, open here a command prompt and, go to the `iac` folder and run the following command:
i. `git init` to initialize local repo
ii. `git add .` to add everything to the git repo
iii. `git commit -m "<your commit message>"` to commit changes
iv. `git remote add origin <Azure DevOps repo url>` to define the remote repo
v. `git push -u origin --all` to push all the code in the remote repo
5. Check in the **Repos** area of the Azure DevOps site that the code is committed correctly
6. Click on **Set up build** button on the right top of the window, it automatically selects the `azure-pipelines.yml` file from the repo and sets up the build. 
**WARNING**: at the moment of writing (Aug 2019) there is a bug that cannot permit to add variables during this phase, so if you don't cancel the first build it will fail. Cancel the first build, click on the 3 dots in the right top and select **Edit pipeline**. You will see a button at the right top **Variables**, click on it and add the following variables:
- TF_STATE_RG = resource group name (see A.2)
- TF_STG_KEY = define a key for the status
- TF_STG_NAME = storage name (see A.2)
7. Run the build, once it will finished you will have the following lists of resources deployed on Azure: an AKS cluster, an ACR, a virtual network, an Event Hub and its Namespace, an Azure Storage

Note that in the **Deploy stage Deploy job** some outputs have been created. Keep them for the next steps.

[not useful in this moment]
- in _Project Settings_ -> _Service Connections_ -> _+ New Service Connection_, select **Docker Registry** to create connection for the Azure Container Registry (this is created with the IaC pipeline <add link>)
