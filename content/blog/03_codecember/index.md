---
title: Deploying my API: first troubles
date: "2022-12-06T08:12:03.284Z"
---
Hello there :)
Now that I've virtualized my Api and uploaded it to Azure Container Registry, I want to deploy it as a Container app.

Here are the commands:

`az login`
`az extension add --name containerapp --upgrade`
`az provider register --namespace Microsoft.App`

```
APP_NAME="my-container-app"
RESOURCE_GROUP="codecember"
LOCATION="francecentral" 
CONTAINERAPPS_ENVIRONMENT="my-environment"
IMAGE="codecembercontainerregistry.azurecr.io/codecember/days-to-xmas:v2"
```
```
az containerapp env create \
  --name $CONTAINERAPPS_ENVIRONMENT \
  --resource-group $RESOURCE_GROUP \
  --location $LOCATION
```
```
az containerapp create \
  --name $APP_NAME \
  --resource-group $RESOURCE_GROUP \
  --environment $CONTAINERAPPS_ENVIRONMENT \
  --image $IMAGE \
  --target-port 80 \
  --ingress 'external' \
  --query properties.configuration.ingress.fqdn
```

Here happens the first issue:
```
(WebhookInvalidParameterValue) The following field(s) are either invalid or missing. Invalid value: "codecembercontainerregistry.azurecr.io/codecember/days-to-xmas:v2": GET https:?scope=repository%3Acodecember%2Fdays-to-xmas%3Apull&service=codecembercontainerregistry.azurecr.io: UNAUTHORIZED: authentication required, visit https://aka.ms/acr/authorization for more information.: template.containers.my-container-app.image.
```

Why that has happened?
Given that creating a Container App from Azure GUI is working fine, I need to understand how to authenticate correctly to my azure container 
registry.

A partial solution to the problem can be found [here](
https://learn.microsoft.com/en-us/azure/app-service/tutorial-custom-container?pivots=container-linux)

See you tomorrow.





