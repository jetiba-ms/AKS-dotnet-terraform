# Configure Open Service Broker for Azure in the AKS cluster

Open Service Broker for Azure is the open source, [Open Service Broker-compatible](https://www.openservicebrokerapi.org/) API server that provisions managed services in the Microsoft Azure public cloud.
With this broker you can deploy manages resources on Azure and applications simultaneously, treating the infrastructure components as if they were deployments.

## OSBA setup
For more details on how to configure OSBA in AKS and samples you can follow the quickstart [here](https://github.com/Azure/open-service-broker-azure/blob/master/docs/quickstart-aks.md#configure-the-cluster-with-open-service-broker-for-azure). Below you can find a short list of the commands for the configuration.

1. Deploy the Service Catalog 
    ```
    helm repo add svc-cat https://svc-catalog-charts.storage.googleapis.com`
    ```
    
    ```
    helm install svc-cat/catalog --name catalog --namespace catalog --set controllerManager.healthcheck.enabled=false --set apiserver.healthcheck.enabled=false --set controllerManager.healthcheck.enabled=false 
    ```

> The Service Catalog is an extension API that provides a way to list, provision, and bind with external Managed Services from Service Brokers without needing detailed knowledge about how those services are created or managed. You can also find more details in [Kubernets docs](https://kubernetes.io/docs/concepts/extend-kubernetes/service-catalog/)

2. Deploy OSBA
    ```
    helm repo add azure https://kubernetescharts.blob.core.windows.net/azure 
    ```
    ```
    helm install azure/open-service-broker-azure --name osba --namespace osba --set azure.subscriptionId="<your_Azure_subscription_id>" --set azure.tenantId="<your_Azure_AD_tenant_id>" --set azure.clientId="<service_principal_client_id>" --set azure.clientSecret="<service_principal_secret>" --set modules.minStability=experimental
    ```
    **<your_Azure_subscription_id>**

    You can find this information open whatever Resource Group on the Azure Portal

    **<your_Azure_AD_tenant_id>**

    a. in _All Services_ search for _Azure Active Directory_

    b. click on it and select _Properties_ in the _Manage_ section of the tab

    c. copy the **Directory ID**


    **<service_principal_client_id>** 

    a. in the same Azure Active Directory tab click on _App Registration_ 

    b. search for the name of your Service Principal and select it

    c. copy the **Application (client) ID** in the _overview_

    **<service_principal_secret>**

    In this case, once after the creation is no more possible to retrieve the secret, so if you don't take note about that, you have to create a new one in this way:

    a. in the same Azure Active Directory tab, select _Certificates & secrets_

    b. click on _New Client Secret_

    c. provide a description and an expiration

    d. click add and copy the secret before exiting from the blade
    
3. Check the status of the installation until every pods will be in _Running_ state
    ```
    kubectl get pods --namespace catalog
    ```
    ```
    kubectl get pods --namespace osba
    ```
    
## Provision resources in Azure
Once everything is up and running you can deploy the managed resources on Azure.
First of all check [here](https://github.com/Azure/open-service-broker-azure#supported-services) the supported services and how to configure the yaml files to proceed with the deployment.
You need 2 yaml files:

- ServiceInstance that describe the Azure resource
- ServiceBinding that will create secret in the AKS cluster in order to communicate with the resource provisioned with the previous file

In the sample that we use here, we need and Azure Event Hub in order to queue messages sent from one of the applications.

**eventhub-instance.yaml**
   ```
   apiVersion: servicecatalog.k8s.io/v1beta1
   kind: ServiceInstance
   metadata:
     name: <eh_resource_on_Azure_name>
     namespace: <ns_name>
   spec:
     clusterServiceClassExternalName: azure-eventhubs
     clusterServicePlanExternalName: <plan could be basic-standard>
     parameters:
       location: <location>
       resourceGroup: <rg_name>
   ```
 
 **eventhub-binding.yaml**
   ```
   apiVersion: servicecatalog.k8s.io/v1beta1
   kind: ServiceBinding
   metadata:
     name: ehdemosba-binding
     namespace: tst
   spec:
     instanceRef:
       name: ehdemosba2
   secretName: demo-osba-eh-secret
   ```

Now, we can create the service via kubectl for the instance `kubectl create -f service-instance.yaml`.

Check if the service is provisioning using `kubetcl get serviceinstance -n <ns_name>`, you can also check directly in the Azure portal.

And for the binding `kubectl create -f service-binding.yaml`.

Check if the service is provisioning using `kubetcl get serviceinstance -n <ns_name>` and `kubetcl get servicebinding -n <ns_name>`.

## A little change to make it work
A last, but not least, thing to do is change a little bit the secret generate from OSBA in order to make it work with the actual images.

1. Run the command `kubectl edit secret/demo-osba-eh-secret -n tst` it opens a notepad window.
2. Looking for the _data_ section
3. change **connectionString** with **EventHubConnectionString**
4. change **eventHubName** with **EventHubName**
5. Close the file and you will see `secret/demo-osba-eh-secret edited` message in the console

So now you are ready to deploy the application, see [Deploy images on AKS using Helm](https://github.com/jetiba-ms/AKS-dotnet-terraform/blob/wikiMaster/%5BManual%5D%20Deploy%20images%20on%20AKS%20using%20Helm.md#deploy-images-on-aks-using-helm).
