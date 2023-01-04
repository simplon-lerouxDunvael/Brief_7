## Etapes scripts et Pipelines

## 1. Creation of a resource group

```bash
az group create --location francecentral --name b7duna
```

## 2. Creation of a storage account (standard GRS)

```bash
az storage account create --name b7dstoracc --resource-group b7duna --sku Standard_GRS
```

## 3. Creation of the AKS Cluster (with SSH keys generated)

```bash
az aks create -g b7duna -n AKSClusterDuna --enable-managed-identity --node-count 2 --enable-addons monitoring --enable-msi-auth-for-monitoring  --generate-ssh-keys
```

## 4. Connecting the AKS Cluster and Azure

```bash
az aks get-credentials --resource-group b7duna --name AKSClusterDuna
```

## 5. Creation of the redis secret

```bash
kubectl create secret generic redis-secret-duna --from-literal=username=devuser --from-literal=password=password_redis_154
```

## 6. Creation of the storage account secret

```bash
kubectl create secret generic storage-secret --from-literal=azurestorageaccountname=b7dstoracc --from-literal=azurestorageaccountkey=Ha/rrRrMwoLotpOK1wT5a1dphjPgfa0z9NZjf7W/1veO6nhHgNtzvjFyIK+y1oBy+I92/y73CPVp+AStu1jQQQ==
```

## 7. Connecting to Azure DevOps Pipelines

First i went to Azure DevOps, created a project and clicked on Pipelines : https://dev.azure.com/dlerouxext/b7duna/_build  
Then i added a service connection (Project settings > service connections > add > kubernetes).

![service_connection2](https://user-images.githubusercontent.com/108001918/210520992-0536c68a-17b6-4b8a-91e4-2bccb2159e75.png)

## 8. Creation of a test pipeline

I created a new pipeline and provided its location (Github.yaml) and the repository (Brief_7). Then i chose a starter template and used the assistant to add a Kubectl task.  
Finally i saved it and ran it. 

![pipeline_job_run](https://user-images.githubusercontent.com/108001918/210521068-ec3cc98c-e2ab-46a7-9d46-3cadd39a3c37.png)

## 9. Checks and tests

In order to understand how the pipeline works and the path used on the pipeline's environment vm, i used the commands ```pwd``` and ```ls -la``` on my pipeline script :

![pipeline_job_run2](https://user-images.githubusercontent.com/108001918/210543908-3f4670ec-8fdb-444f-acb7-e728a63d2d48.png)

It allowed me to know which path i needed to put to refer the .yaml file to use in the pipeline.

## 10. Error messages

I received an error at the end of the job. It seems that Azure does not have the rights to create a persistent volume and the PV claim.

![Error_pipeline](https://user-images.githubusercontent.com/108001918/210544375-1f1e042e-a659-4c1f-941b-49a9ce07d471.png)


On the Azure CLI i searched the service account default used by Azure to run the pipeline with the following command :

```bash
kubectl get serviceaccounts/default
```

Alfred tried to bind an admin role he created to the Azure service account Default to see if we could get admin rights on the Kubernetes cluster. Sadly it did not work.

![Error_pipeline2](https://user-images.githubusercontent.com/108001918/210545569-b0ce0e74-e461-4407-b3fc-69c6ecfbaad5.png)

## 11. Trying to find a solution

I recreated my Kubernetes Service Connection and checked "Use cluster admin credentials". Then i reran my pipeline.

I created a container in my storage account in order to be able to use my fileshare for the PV and PVC.

My PV displayed but was not mounted thus my redis container could not be created.

Get the logs and events to understand the errors :

```bash
kubectl get events
```

```bash
kubectl get events --sort-by='.metadata.creationTimestamp'
```

```bash
kubectl get events --sort-by='.metadata.creationTimestamp' -w
```

## 12. Updating the voting app on the script

I changed the previous version of the voting app with the new one : simplonasa/azure_voting_app:v1.0.11 and my container for the Voting app was successfully created.
It then displayed in CrashLoopBackOff because redis was not created but now i just needed to find a solution for redis for everything to work properly.


## X. Creation of DNS records (A)

__Add screenshot__

## X. Add Gandi webhook jetstack with helm

[Jetstack](https://github.com/bwolf/cert-manager-webhook-gandi)

```bash
helm repo add jetstack https://charts.jetstack.io
```

```bash
helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --set installCRDs=true --version v1.9.1 --set 'extraArgs={--dns01-recursive-nameservers=8.8.8.8:53\,1.1.1.1:53}'
```

## X. Gandi secret

```bash
kubectl create secret generic gandi-credentials --namespace cert-manager --from-literal=api-token='2DqJpnKJljl9yWQIolq2xRXO'
```

## X. Install cert-manager webhook for gandi

```bash
helm install cert-manager-webhook-gandi --repo https://bwolf.github.io/cert-manager-webhook-gandi --version v0.2.0 --namespace cert-manager --set features.apiPriorityAndFairness=true  --set logLevel=6 --generate-name
```

## X. create secret role and bind for webhook

```bash
kubectl create role access-secret --verb=get,list,watch,update,create --resource=secrets
```

```bash
kubectl create rolebinding --role=access-secret default-to-secrets --serviceaccount=cert-manager:cert-manager-webhook-gandi-1665664967
```

Then apply in this order : ingress -> issuer -> certificate

## X. 
## X. 



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

