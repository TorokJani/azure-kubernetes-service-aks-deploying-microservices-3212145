# Check you still have ui container image from earlier task
docker images

# Connect to Azure
az login

# Login to ACR
az acr login --name <acrName>

az acr credential show --name acr27699
anosorok@EPHUBUDW05B5:~$ az acr credential show --name acr27699
Run 'az acr update -n acr27699 --admin-enabled true' to enable admin first.
az acr update -n acr27699 --admin-enabled true

docker login acr27699.azurecr.io -u <username> -p <password>

# Get ACR server address
az acr list --resource-group aks-rg --query "[].{acrLoginServer:loginServer}" --output table

# Tag the local copy of the UI Container Image
docker tag ui:v1.0 acr27699.azurecr.io/ui:v1.0

# Confirm new UI image with acr tag exist
docker images | grep ui -w

# Push image to ACR
docker push <acrLoginServer>/ui:v1.0
docker push acr27699.azurecr.io/ui:v1.0

# Verify the image is in ACR
az acr repository list --name <acrName> --output table
az acr repository list --name acr27699.azurecr.io --output table

# Connect to AKS
az aks get-credentials --resource-group <resourceGroupName> --name <AKSClusterName>

# Verify that you are connected
kubectl get nodes

# Apply UI Manifest to the AKS cluster
kubectl apply -f demo-ui.yaml

# Verify pod is deployed for UI
kubectl get pods -n ns-posthub--env

# See every related pod/service/deployment etc in this namespace
kubectl get all -n ns-posthub--env

# Describe a UI pod
kubectl describe pod <uiPodName> -n ns-posthub--env
kubectl describe pod ui--env-6557c779fb-d7fjg -n ns-posthub--env
ui--env-6557c779fb-d7fjg

