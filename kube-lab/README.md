# Kubernetes: Hands On With Microservices

*Expected timings: 115 mins (1hr 55 mins) for Intro and Modules 1 - 6*

Hands-on lab URL: https://azurecitadel.github.io/labs/kubernetes/

## Instructions

Follow along with the Kubernetes lab (https://azurecitadel.github.io/labs/kubernetes/) and refer to these notes for any additional info or differences to the way we will run the lab.  The main difference will be that we will run all commands from the [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).

## Pre-requisites:

* Azure Subscription

## Assumptions:

* Use Azure Cloud Shell in browser

## Initial steps:

* Login into Azure portal (https://portal.azure.com) with your provided subscription details
* Open/setup Azure Cloud Shell by click the shell icon on the top toolbar
* Use bash shell (not powershell)
* Verify your subscription is selected (type: `az account show`)
* Open the follow URL in another browser tab: https://azurecitadel.github.io/labs/kubernetes/
* Start the labs modules!

## Intro, setup, etc. - 10 mins

[Direct link to intro](https://azurecitadel.github.io/labs/kubernetes/)

Go with "Option 2: Azure Cloud Shell" and that now includes VSCode (albeit simplified, e.g. no multiple tabs)

## Kubernetes: Module 1 - Deploying Kubernetes - 20 mins

[Direct link to module 1](https://azurecitadel.github.io/labs/kubernetes/part1/)

### Registering Providers

Skip "Registering Providers" section as your subscription should have these registered.

### Deploying AKS

Use `australiaest` for the region

```
group=kube-lab
region=australiaeast
```

Note: If you close/exit the cloud shell or the browser tab you'll loose these variables and youll need to set them again.

One way to retain them and have them load automatically, is:

```
echo "export group=kube-lab" >> ~/.bashrc
echo "export rehion=australiaeast" >> ~/.bashrc
source ~/.bashrc
```

Next time you open the shell the values will be automatically "sourced" or set.

The VM size `DS2_v2` is 2 cores (vCPU), 7 GiB RAM (see: https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-general#dsv2-series)

Now wait for the 3 VMs to be created…will take about 15 mins.
Use this wait time to familiarise yourselves with the tutorial, try reading some of the links presented.

Skip section "Get Kubectl CLI tool (WSL Bash Only)" and from now on any section that says "WSL Bash Only" snice we are using Azure Cloud Shell.

Before opening the kubenetes dashboard, you need to grant authorisation since our clusters is RBAC enabled.  Run: 

```
kubectl create clusterrolebinding kubernetes-dashboard -n kube-system --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

Then open the kubernetes dashboard. CTRL+C to exit the dashboard.

## Kubernetes: Module 2 - Azure Container Registry (ACR) - 20 mins

[Direct link to module 2](https://azurecitadel.github.io/labs/kubernetes/part2/)

The other way (instead of creating an image pull secret) to grant AKS access to your private registry is to grant access to the service principle used by AKS.  This is explained in the lab  notes.

## Kubernetes: Module 3 - Deploying the Data Layer - 15 mins

[Direct link to module 3](https://azurecitadel.github.io/labs/kubernetes/part3/)

### Deploy Data API

When waiting on your deployment, you can watch for pod updates like so:

```
kubectl get pods -w
```

Press `CTRL+C` to exit from the watch mode.

### Access Smilr Data API

You can't access the Smilr Data API from outside the Azure Cloud Shell session.  You'll need to run the port forward step like so:

```
kubectl port-forward data-api-695ddc577-xr9qc 8080:4000
```

(use your pod name which will be different)

Then press `CTRL+Z` to break to the shell then type `bg` to put the forwarding process into the background and let it continue running.

Now access the Data API:

```
curl localhost:8080/api/info | jq .
```

(The `| jq .` bit at the end will format and colour the JSON output)

To return to the port forwarding, type `fg` and press ENTER.
Then press `CTRL+C` to stop the port forwarding.

## Kubernetes: Module 4 - Services & Networking - 15 mins

[Direct link to module 4](https://azurecitadel.github.io/labs/kubernetes/part4/)

### Create Service for Data API

It can a few mins for the pubic IP to be assigned to the service. You can watch for updated like so:

```
kubectl get service -w
```

Then press `CTRL+C` to exit the watch.

Once you see the IP, you can access the service in your browser with that ip: http://<pubic_ip>/api/info

## Kubernetes: Module 5 - Deploying the Frontend - 15 mins

[Direct link to module 5](https://azurecitadel.github.io/labs/kubernetes/part5/)

### Create Demo Data

After loading the demo data, refresh the Smilr frontend in your browser to see some mock data.

## Kubernetes: Module 6 - Scaling & Persistence - 20 mins

[Direct link to module 6](https://azurecitadel.github.io/labs/kubernetes/part6/)

### Scale Frontend

Optional: After scaling the pods, you could try the Kubernetes Dashboard again to see what that shows.

```
az aks browse -g $group -n aks-cluster --enable-cloud-console-aks-browse
```

## Kubernetes: Extra Optional Exercises

[Direct link to optional exercises](https://azurecitadel.github.io/labs/kubernetes/extra/)

You can try out any of the optional exercises on your own.

### Extra Exercise 1 - Ingress Routing, Ingress Controllers, Cert-Manager

If you prefer, you can try this extra exercise (not part of the linked kubernetes lab) which will introduce the Ingress object and TLS termination so you can get a friendly URL for your Smilr app and have that endpoint run over SSL/TLS and be trusted by the browser!

Follow the instructions at [Create an HTTPS ingress controller on Azure Kubernetes Service (AKS)](https://docs.microsoft.com/en-us/azure/aks/ingress-tls) as a guide and adapt as necessary for your Smilr app.

### Extra Exercise 2 - Deploy to AKS via Azure DevOps Pipelines

Sign up for Azure DevOps - https://dev.azure.com

Create a new organisation and project.

Import the GitHub repo (https://github.com/benc-uk/smilr) into an Azure Repo.

* Set up a build for the `frontend` container image.
* Push this image to your Azure Container Registry.
* Setup a Release pipeline to deploy the frontend service via a helm chart (one exists in the git repo)
* Write a simple post deploy test to verify the frontend was successfully deployed

Import 

Good luck!


