## Useful commands with Kubernetes (on Azure)

Get cluster credential to connect from local `az aks get-credentials -n <cluster-name> -g <rg-name>`

Get clusert information, as master, metrics server and dashboard endpoints `kubectl cluster-info`

Get all namespaces `kubectl get all --all-namespaces`

Get deployments `kubectl get deployments -n <ns_name>`

Get pods `kubectl get pods -n <ns_name>`

Describe an object `kubectl describe <object_name>/<object_id> -n <ns_name>`, where _object_name_ could be deployment, pod, secret or another object in Kubernetes

Get logs of one pods `kubectl logs <pod_id> -n <ns_name>`
