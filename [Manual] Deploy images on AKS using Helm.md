## Deploy images on AKS using Helm

For each application that you want to deploy, run the following command

```
helm install <chart_name> --namespace <ns_name> --set image.repository=<acr_name>.azurecr.io/<image_name> --set image.tag=<tag>
```
**NB** Remember to check in the values.yaml of the Helm Chart what secrets are required to run well the application.
For example, in the _msgsenderapi_ you have to add `--set secrets.name=demo-osba-eh-secret` to the command above.
