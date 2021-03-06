## Setup Azure Container Registry for Helm Charts

Create Azure Container Registry `az acr create -n <acr_name> -g <rg_name> --sku <basic-standard-premium> --admin-enabled true`

Push image from local to the registry
1. Login to ACR `az acr login --name <acr_name>` or `docker login <acr_loginserver>`, this will promt you for the password, that you can find the password to access in the Azure Portal, as in the image below

![acr_psw.jpg](/.attachments/acr_psw.jpg)

2. Build the image locally `docker build -t <acr_name>.azurecr.io/<image_name>:<tag> .`
3. Push the image `docker push <acr_name>.azurecr.io/<image_name>:<tag>`

Do these steps for all the application you want to build as docker images.

In order to deploy images in the ACR on AKS, it is needed to configure a role in the ACR to allow Helm installed in the AKS cluster to pull images from the registry. In order to do that, as in the picture below:

![acr_add_role.jpg](/.attachments/acr-add-role.jpg)

1. Open _Acces Control (IAM)_ tab in the ACR resource on Azure Portal
2. Click on _Add_ button and select _Add role assignment_
3. In _Role_ select _AcrPull_
4. Select the right Service Principal -> **NB** if you create automatically the Service Principal during the AKS creation, you can find this information run the following command `az aks show --name <cluster_name> --resource-group <rg_name>` and look for _ServicePrincipal_

5. Click _Save_ and close the tab
