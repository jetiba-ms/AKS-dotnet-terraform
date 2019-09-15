
During this phase you will prepare the environment for deploying your containerized applications, using Terraform to manage the infrastructure and create Azure resources.
The following steps are required just once, in a initial phase of the project.

After, you can modify the infrastructure, adding or deleting resources definition in the `main.tf` file. Terraform keeps status in a blob storage, checks what resources are already created on Azure, and applies the changes that have been made.

0 - First of all, clone or download the code from the following link [AKS-dotnet-terraform-iac](https://github.com/jetiba-ms/AKS-dotnet-terraform-iac)

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

C - On Azure Subscription:
1. Go in the Resource Group that has been created from the previous pipeline, and select the ACR resource
2. In the options panel, select _Access Control (IAM)_ -> _Add_ -> _Add role assignment_ and fill the form in the tab on the right as per the image below (changing the name of SP with yours)
![sp-img.jpg](/.attachments/sp-img-4aef24c4-7e69-4194-a6f9-7ce3263e7755.jpg)
**WARNING**: you have to complete these steps manually due to some security considerations on how to manage Service Principals. In fact, if you want to assign this role programmatically, inside the Terraform script, you have to make the SP owner of the entire subscription, and this isn't suggested in term of security and roles segmentation. The best practice in these terms is to create 2 different SPs:
- the first as a contributor of the entire subscription, and used to create the infrastructure
- the second for the cluster, with the roles that are suggested in [the documentation](https://docs.microsoft.com/en-us/azure/aks/best-practices)
In order to keep things easy, in this project we used one Service Principal for everything (at least in this previous version).

Note that in the **Deploy stage Deploy job** some outputs have been created. Keep them for the next steps.

Coming soon: script to delete the cluster and other related resources when you no more need those.
