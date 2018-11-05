# Demo 3 - Running a container in Azure Kubernetes Service

## Prerequisites:

* An AKS cluster already created
* `kubectl` credentials configured for cluster

## Examine YAML manifests

* `webapp.deploy.yaml`
* `webapp.service.yaml`

## Check AKS cluster

```
az aks list -o table
```

## Verify corrext kubectl context is selected

```
kubectl config get-contexts
```

## Deploy container

```
kubectl apply -f webapp.deploy.yaml
```

## Expose as service via Load Balancer

```
kubectl apply -f webapp.service.yaml
```

## Wait for resources to be provisioned

```
# CTRL+C to stop watching
kubectl get pods -l app=webapp -w

# CTRL+C to stop watching
kubectl get service/webapp-svc -w
```

## Test web app

    http://<external_ip>:8888

## Scale replicas

```
kubectl scale --replicas=3 deployment/webapp-deploy
```

(Or edit the YAML file and `kube apply -f webapp.deploy.yaml`)

```
kubectl get pods -l app=webapp
```

## Kill a pod

```
kubectl delete <pod_name>

kubectl get pods -l app=webapp
```

## Test web app again

    http://<external_ip>:8888

## Cleanup

```
kubectl delete service/webapp-svc
kubectl delete deployment/webapp-deploy
```