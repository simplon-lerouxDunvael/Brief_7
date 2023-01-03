## To create an alias for a command on azure CLi 
alias [WhatWeWant]="[WhatIsChanged]"  
*Example :*  
`alias k="kubectl"`  

## To deploy resources with yaml file
kubectl apply -f [name-of-the-yaml-file]  
*Example :*  
`kubectl apply -f azure-vote.yaml`  

## To check resources
kubectl get nodes  
kubectl get pods  
kubectl get services  
kubectl get deployments  
kubectl get events  
kubectl get secrets  
kubectl get logs  
*To keep verifying the resources add --watch at the end of the command :*  
*Example :*  
`kubectl get services --watch`  
*To check the resources according to their namespace, add --namespace after the command and the namespace's name :*  
*Example :*  
`kubectl get services --namespace [namespace's-name]`

## To describe resources
kubectl describe nodes  
kubectl describe pods  
kubectl describe services # or svc  
kubectl describe deployment # or deploy  
kubectl describe events  
kubectl describe secrets  
kubectl describe logs  
*To specify which resource needs to be described just put the resource ID at the end of the command.*   
*Example :*  
`kubectl describe svc redis-service`  
*To access to all the logs from all containers :*  
`kubectl logs podname --all-containers`  
*To access to the logs from a specific container :*  
`kubectl logs podname -c [container's-name]`  
*To list all events from a specific pod :*   
`kubectl get events --field-selector [involvedObject].name=[podsname]`  

## To delete resources
kubectl delete deploy --all  
kubectl delete svc --all  
kubectl delete pvc --all  
kubectl delete pv --all  
az group delete --name [resourceGroupName] --yes --no-wait  

## To create a repository Helm and install Jetstack  
*To create the repository and install Jetstack :*  
`helm repo add jetstack https://charts.jetstack.io`  
*To check the repository created and Jetstack version :*  
`helm search repo jetstack`  

## To create a role for Gandi's secret and bind it to the webhook  
*To create the role :*  
kubectl create role [role-name] --verb=[Authorised-actions] --resource=[Authorised-resource]  
*Example :*  
`kubectl create role access-secrets --verb=get,list,watch,update,create --resource=secrets`  
*To bind it :*  
kubectl create rolebinding --role=[role-name] [role-name] --serviceaccount=[group]:[group-item]  
*Example :*  
`kubectl create rolebinding --role=access-secrets default-to-secrets --serviceaccount=cert-manager:cert-manager-webhook-gandi-1665665029`

## To check TLS certificate in request order
kubectl get certificate  
kubectl get certificaterequest  
kubectl get order  
kubectl get challenge  

## To describe TLS certificate in request order
kubectl describe certificate  
kubectl describe certificaterequest  
kubectl describe order  
kubectl describe challenge  

## Get the IP address to point the DNS to nginx
k get ingress  

## Activate the autoscaler on an existing cluster
az aks update --resource-group b6duna --name AKSClusterd2 --enable-cluster-autoscaler --min-count 1 --max-count 8

## To check the auto scaling creation
get HorizontalPodAutoscaler  
*Example of how the results will display :*  
`horizontalpodautoscaler.autoscaling/scaling-voteapp created`

## To check Webhook configuration
kubectl get ValidatingWebhookConfiguration -A

## Delete Webhook configuration for a role  
kubectl delete -A ValidatingWebhookConfiguration [rolename]  
*Example :*  
`kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission`  