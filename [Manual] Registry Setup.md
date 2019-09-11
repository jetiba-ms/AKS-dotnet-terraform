## Setup Azure Container Registry for Helm Charts

Create Azure Container Registry `az acr create -n <acr_name> -g <rg_name>`

Push image from local to the registry
1. Login to ACR `az acr login --name <acr_name>`
2. Build the image locally `docker build -t <acr_name>.azurecr.io/<image_name>:<tag> .`
3. Pull the image `docker pull <acr_name>.azurecr.io/<image_name>:<tag>`

Do these steps for all the application you want to build as docker images.
