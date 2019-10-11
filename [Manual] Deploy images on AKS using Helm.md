## Deploy images on AKS using Helm

Follow the commands below to deploy applications.

### msgsenderapi

```
helm install msgsenderapi --namespace <ns_name> --set image.repository=<acr_name>.azurecr.io/msgsenderapi --set image.tag=latest --set secrets.name=demo-osba-eh-secret
```

### msgprocessor

```
helm install msgprocessor --namespace <ns_name> --set image.repository=<acr_name>.azurecr.io/msgsenderapi --set secrets.name=<choose-a-name-for-a-new-secret> --set secrets.ehconnstring=<event_hub_connection_string> --set secrets.ehname=<event_hub_name> --set secrets.stgconnstring=<storage_connection_string> --set secrets.stgcontainername=<storage_name>
```
We have to create a new secret, because the one created with OSBA is not complete to work with the already available application (so, it is just to don't have to change some code).

To check if the deployment is done use `kubectl get pods -n tst`.
![cmd-pod-img.jpg](/.attachments/cmd-pod-img-33fc4901-c007-4393-91dc-60d504c25b77.jpg)

### Test
1. `kubectl get service -n <namespace>` to see the external IP of the msgsenderapi services to send new messages to the event hub and check if everything is working well
![cmd-ip-img.jpg](/.attachments/cmd-ip-img-d868916d-7e21-4d38-9194-118ac5761354.jpg)
3. Copy the IP, open Postman and send an HTTP POST to `http://<IP address>/api/value`s with as a body something like `{"values": "my-message-here"}`, if it is working well it returns **200 OK** response
4. In the command prompt run `kubectl logs -f <pod-name> -n <namespace>` and see the message processed 
![msgproc-img.jpg](/.attachments/msgproc-img-65471a5d-ac67-44cc-ad8c-06e254e7ded9.jpg)
