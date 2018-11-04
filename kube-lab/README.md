Kubernetes: Hands On With Microservices

Expected timings: 115 mins (1hr 55 mins) for Intro and Modules 1 - 6

URL: https://azurecitadel.github.io/labs/kubernetes/

Hands-on exercise test drive and notes

Pre-requisites:
	- Azure Subscription

Assumptions:
	- Use Azure Cloud Shell in browser

Initial steps:
	- Login into portal
	- Open/setup Azure Cloud Shell
	- Use bash shell
	- Verify your subscription is selected (az account show)
	- Visit: https://azurecitadel.github.io/labs/kubernetes/

Start labs:

Intro, setup, etc. - 10 mins

Go with "Option 2: Azure Cloud Shell" and that now includes VSCode (albeit simplified, e.g. no multiple tabs)

Kubernetes: Module 1 - Deploying Kubernetes - 20 mins

Registering Providers

Skip "Registering Providers" section as your subscription should have these registered.

Deploying AKS

Use "australiaest" for the region

group=kube-lab
region=australiaeast

Note: If you close/exit the cloud shell or the browser tab you'll loose these variables and youll need to set them again.

One way to retain them and have them load automatically, is:

echo "export group=kube-lab" >> ~/.bashrc
echo "export rehion=australiaeast" >> ~/.bashrc
source ~/.bashrc

Next time you open the shell the values will be automatically "sourced" or set.

The VM size DS2_v2 is 2 cores (vCPU), 7 GiB RAM (see: https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-general#dsv2-series)

Now wait for the 3 VMs to be created…will take about 15 mins.
Use this wait time to familiarise yourselves with the tutorial, try reading some of the links presented.

Skip section "Get Kubectl CLI tool (WSL Bash Only)" and from now on any section that says "WSL Bash Only" snice we are using Azure Cloud Shell.

Before opening the kubenetes dashboard, you need to grant authorisation since our clusters is RBAC enabled.  Run: 

kubectl create clusterrolebinding kubernetes-dashboard -n kube-system --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard

Then open the kubernetes dashboard. CTRL+C to exit the dashboard.

Kubernetes: Module 2 - Azure Container Registry (ACR) - 20 mins

The other way (instead of creating an image pull secret) to grant AKS access to your private registry is to grant access to the service principle used by AKS.  This is explained in the lab  notes.

Kubernetes: Module 3 - Deploying the Data Layer - 15 mins

Deploy Data API

When waiting on your deployment, you can watch for pod updates like so:

kubectl get pods -w

Press CTRL+C to exit from the watch mode.

Access Smilr Data API

You can't access the Smilr Data API from outside the Azure Cloud Shell session.  You'll need to run the port forward step like so:

kubectl port-forward data-api-695ddc577-xr9qc 8080:4000

(use your pod name which will be different)

Then press "CTRL+Z" to break to the shell then type "bg" to put the forwarding process into the background and let it continue running.

Now  access the Data API:

curl localhost:8080/api/info | jq .

(The `| jq .` bit at the end will format and colour the JSON output)

To return to the port forwarding, type "fg" and press ENTER.
Then press "CTRL+C" to stop the port forwarding.

Kubernetes: Module 4 - Services & Networking - 15 mins

Create Service for Data API

It can a few mins for the pubic IP to be assigned to the service. You can watch for updated like so:

kubectl get service -w

Then press CTRL+C to exit the watch.

Once you see the IP, you can access the service in your browser with that ip: http://<pubic_ip>/api/info

Kubernetes: Module 5 - Deploying the Frontend - 15 mins

Create Demo Data

After loading the demo data, refresh the Smilr frontend in your browser to see some mock data.

Kubernetes: Module 6 - Scaling & Persistence - 20 mins

Scale Frontend

Optional: After scaling the pods, you could try the Kubernetes Dashboard again to see what that shows.

az aks browse -g $group -n aks-cluster --enable-cloud-console-aks-browse

Kubernetes: Extra Optional Exercises

You can try out any of the optional exercises on your own.

If you prefer, you can try this extra one which will introduce the Ingress object and TLS termination so you can get a friendly URL for your Smilr app and have that endpoint run over SSL/TLS and be trusted by the browser!

Ingress and Ingress Controllers



Add TLS with Cert Manager

