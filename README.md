# GitOps Workflow with ArgoCD

This repository demonstrates deploying a Flask application on Kubernetes using GitOps practices with ArgoCD.

## Prerequisites

* [kind](https://kind.sigs.k8s.io/)
* [Docker](https://docs.docker.com/get-docker/)
* [kubectl](https://kubernetes.io/docs/tasks/tools/)

---

## Setup

* Create the cluster
```shell
kind create cluster --name gitops --config kind-cluster.yaml
````


* Check if the cluster is up and nodes are ready

```shell
kubectl get nodes
```


* Build the application image

```shell
docker build -t app:latest .
```

* Load the Docker image into the kind cluster

```shell
kind load docker-image app:latest --name gitops
```



* Install ArgoCD (create namespace and install)

```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

* Check if ArgoCD pods are running

```shell
kubectl get pods -n argocd
```



* Access the ArgoCD UI

```shell
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Then open [http://127.0.0.1:8080](http://127.0.0.1:8080) in your browser.
Username: `admin`

* Get the initial admin password

```shell
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d; echo
```



* Deploy the ApplicationSet with ArgoCD

```shell
kubectl apply -f app.set.yaml
```



* Verify the deployment in the ArgoCD UI or by checking pods

```shell
kubectl get pods -n dev
```
