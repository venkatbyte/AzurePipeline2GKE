# AzurePipeline2GKE
This repository will expalin how to create the service connection in Azure Devops to Google Cloud Engine (GKE) to deploy any microservices
## 1. Create Service Account to manange the gke cluster externally.
```
kubectl create serviceaccount **venkatbyte-pipelines**
```
## 2. Create generic secret toekn for service account to integrate with Azure Pipelines  
```
kubectl create secret generic **venkatbyte-pipelines-token** \
  --type=kubernetes.io/service-account-token \
  --dry-run=client -o yaml | \
kubectl annotate -f - --local -o yaml \
  kubernetes.io/service-account.name=**venkatbyte-pipelines** | \
kubectl apply -f -
```
## 3. Assign the cluster admin role to the service account 
```
kubectl create clusterrolebinding **venkatbyte-pipelines** --clusterrole=cluster-admin --serviceaccount=**default:venkatbyte-pipelines**
```
## 4. Create Kubernetes Cluster in CGP
Once you create kubernetes cluster in CGP either through Autopilot or Standard method, run below command to get cluster name.  
```
gcloud container clusters list
```
## 5. Run below command to get IP address of the cluster
```
gcloud container clusters describe **venkatbyte-cluster** --format=value\(endpoint\) --region=**us-east1**
```
## 6. Get the secret by running below command
```
kubectl get secret **venkatbyte-pipelines-token** -o json
```
