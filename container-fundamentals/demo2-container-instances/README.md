# Demo 1 - Running a container in Azure Container Instances

## Pre-requisities

* Uses previously built image in demo 1 (or just use pre pushed image: `azurecontainerdemos/webapp-linux:v1.0.0`)

## Create Resource Group

```
az group create --name container-demos --location australiaeast
```

## Create the Container in ACI

```
az container create --resource-group container-demos --name webapp --image azurecontainerdemos/webapp-linux:v1.0.0 --dns-name-label cb-aci-webapp --ports 8000
```

## Get FQDN and Provisioning State

```
az container show --resource-group container-demos --name webapp --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
```

## Browse to containerised web app

(Use your URL which will be different)

http://cb-aci-webapp.australiaeast.azurecontainer.io:8000/

## View logs

```
az container attach --resource-group container-demos -n webapp
```

## List container instances

```
az container list --resource-group container-demos --output table
```

## Cleanup

```
az container delete --resource-group container-demos --name webapp
```