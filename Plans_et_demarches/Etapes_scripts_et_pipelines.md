### Etapes scripts et Pipelines

### 1. Creation of a resource group

```bash
az group create --location francecentral --name b7duna
```

### 2. Creation of a storage account (standard GRS)

```bash
az storage account create --name b7dstoracc --resource-group b7duna --sku Standard_GRS
```

### 3. Creation of the AKS Cluster (with SSH keys generated)

```bash
az aks create -g b7duna -n AKSClusterDuna --enable-managed-identity --node-count 2 --enable-addons monitoring --enable-msi-auth-for-monitoring  --generate-ssh-keys
```

### 4. Connecting the AKS Cluster and Azure

```bash
az aks get-credentials --resource-group b7duna --name AKSClusterDuna
```

### 5. Creation of the redis secret

```bash
kubectl create secret generic redis-secret-duna --from-literal=username=devuser --from-literal=password=password_redis_154
```

### 6. Creation of the storage account secret

```bash
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=b7dstoracc --from-literal=azurestorageaccountkey=Ha/rrRrMwoLotpOK1wT5a1dphjPgfa0z9NZjf7W/1veO6nhHgNtzvjFyIK+y1oBy+I92/y73CPVp+AStu1jQQQ==
```

### 7. Connecting to Azure DevOps Pipelines

### X. Creation of DNS records (A)

__Add screenshot__

### X. Add Gandi webhook jetstack with helm

[Jetstack](https://github.com/bwolf/cert-manager-webhook-gandi)

```bash
helm repo add jetstack https://charts.jetstack.io
```

```bash
helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --set installCRDs=true --version v1.9.1 --set 'extraArgs={--dns01-recursive-nameservers=8.8.8.8:53\,1.1.1.1:53}'
```

### X. Gandi secret

```bash
kubectl create secret generic gandi-credentials --namespace cert-manager --from-literal=api-token='2DqJpnKJljl9yWQIolq2xRXO'
```

### X. Install cert-manager webhook for gandi

```bash
helm install cert-manager-webhook-gandi --repo https://bwolf.github.io/cert-manager-webhook-gandi --version v0.2.0 --namespace cert-manager --set features.apiPriorityAndFairness=true  --set logLevel=6 --generate-name
```

### X. create secret role and bind for webhook

```bash
kubectl create role access-secret --verb=get,list,watch,update,create --resource=secrets
```

```bash
kubectl create rolebinding --role=access-secret default-to-secrets --serviceaccount=cert-manager:cert-manager-webhook-gandi-1665664967
```

Then apply in this order : ingress -> issuer -> certificate

### X. 
### X. 



-----
## __USEFULL COMMANDS__

#### update AKS with autoscale
```bash
az aks update --resource-group b7duna --name KlusterDuna --enable-cluster-autoscaler --min-count 1 --max-count 8
```
#### Autoscaling

[Autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)

[Autoscaling Walkthrough](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/)

----

## To create an alias for a command on azure CLi 
alias [WhatWeWant]="[WhatIsChanged]"  
*Example :*  
```bash
alias k="kubectl"
```

## To deploy resources with yaml file
kubectl apply -f [name-of-the-yaml-file]
*Example :*  
```bash
kubectl apply -f azure-vote.yaml
```

## To check resources
```bash
kubectl get nodes
kubectl get pods
kubectl get services
kubectl get deployments
kubectl get events
kubectl get secrets
kubectl get logs
```
*To keep verifying the resources add --watch at the end of the command :*
*Example :*
```bash
kubectl get services --watch
```
*To check the resources according to their namespace, add --namespace after the command and the namespace's name :*
*Example :*
```bash
kubectl get services --namespace [namespace's-name]
```

## To describe resources
```bash
kubectl describe nodes
kubectl describe pods
kubectl describe services # or svc
kubectl describe deployment # or deploy
kubectl describe events
kubectl describe secrets
kubectl describe logs
```
*To specify which resource needs to be described just put the resource ID at the end of the command.*
*Example :*
```bash
kubectl describe svc redis-service
```
*To access to all the logs from all containers :*
```bash
kubectl logs podname --all-containers
```
*To access to the logs from a specific container :*
```bash
kubectl logs podname -c [container's-name]
```
*To list all events from a specific pod :*
```bash
kubectl get events --field-selector [involvedObject].name=[podsname]
```

## To delete resources
```bash
kubectl delete deploy --all
kubectl delete svc --all
kubectl delete pvc --all
kubectl delete pv --all
az group delete --name [resourceGroupName] --yes --no-wait
```

## To create a repository Helm and install Jetstack
*To create the repository and install Jetstack :*
```bash
helm repo add jetstack https://charts.jetstack.io
```
*To check the repository created and Jetstack version :*
```bash
helm search repo jetstack
```

## To create a role for Gandi's secret and bind it to the webhook
*To create the role :*
```bash
kubectl create role [role-name] --verb=[Authorised-actions] --resource=[Authorised-resource]
```
*Example :*  
```bash
kubectl create role access-secrets --verb=get,list,watch,update,create --resource=secrets
```
*To bind it :*  
```bash
kubectl create rolebinding --role=[role-name] [role-name] --serviceaccount=[group]:[group-item]
```
*Example :*  
```bash
kubectl create rolebinding --role=access-secrets default-to-secrets --serviceaccount=cert-manager:cert-manager-webhook-gandi-1665665029
```

## To check TLS certificate in request order
```bash
kubectl get certificate
kubectl get certificaterequest
kubectl get order
kubectl get challenge
```

## To describe TLS certificate in request order
```bash
kubectl describe certificate
kubectl describe certificaterequest
kubectl describe order
kubectl describe challenge
```

## Get the IP address to point the DNS to nginx
```bash
k get ingress
```

## Activate the autoscaler on an existing cluster
```bash
az aks update --resource-group b6duna --name AKSClusterd2 --enable-cluster-autoscaler --min-count 1 --max-count 8
```

## To check the auto scaling creation
```bash
get HorizontalPodAutoscaler
```
*Example of how the results will display :*  
```bash
horizontalpodautoscaler.autoscaling/scaling-voteapp created
```

## To check Webhook configuration
```bash
kubectl get ValidatingWebhookConfiguration -A
```

## Delete Webhook configuration for a role
```bash
kubectl delete -A ValidatingWebhookConfiguration [rolename]  
```
*Example :*  
```bash
kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission
```