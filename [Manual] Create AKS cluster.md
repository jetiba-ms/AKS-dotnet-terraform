## Manual cluster setup
### Login in Azure with cmd 

`az login`
  
### Create a new Resource Group on Azure 

`az group create --name <rg_name> --location <rg_location>`

### Service Principal setup

`az ad sp create-for-rbac --skip-assignment` create the SP

`az role assignment create --assignee <appId> --scope <resourceScope> --role Contributor` assign a particular role/permission to the SP
For example, you can use `--scope /subscriptions/<subscription_id>` and `--role Contributor` to make the SP contributor of the entire subscription
  
### Cluster creation 

`az aks create --resource-group <rg_name> --name <cluster_name> --node-count 1 --enable-addons monitoring,http_application_routing --generate-ssh-keys --service-principal <appId> --client-secret <password>` 
	
### Install kubectl locally 

`az aks install-cli`
	
### Get access credential for the cluster (the information will be stored in kube.config file) 

`az aks get-credentials --resource-group <rg_name> --name <cluster_name>`
	
### To test cluster access 

`kubectl cluster-info`
	
### Create RBAC role for helm 

`kubectl create -f https://raw.githubusercontent.com/Azure/helm-charts/master/docs/prerequisities/helm-rbac-config.yaml`
	
### Initialize helm server in the cluster 

`helm init --service-account tiller`
	
### Check Helm installation 

`kubectl get pod --all-namespaces`
	
### Enable Kubenernetes dashboard for RBAC enabled cluster 

`kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard`
	
### Create local tunnel on localhost to access the remote cluster dashboard 

`az aks browse --resource-group <rg_name> --name <cluster_name>`
