# Kubernetes & Kubeadm

Kubeadm is a tool built to provide `kubeadm init` and `kubeadm join` as best-practice fast paths for creating Kubernetes clusters

## 1. Cluster Setup

### 1.1. Namespace

In Kubernetes, namespaces provide a mechanism for isolating groups of resources within a single cluster. Names of resources need to be unique within a namespace, but not across namespaces.

```sh
# list all existed namespace
kubectl get namespace

# create a namespace for PostgreSQL
kubectl create namespace pg

# set current namespace for all subsequent kubectl commands
kubectl config set-context --current --namespace=pg

# validate current namespace
kubectl config view --minify | grep namespace
```

Alternatively, a namespace can be defined in a YAML file and then applied

```yml
apiVersion: v1
kind: Namespace
metadata:
  name: pg
```

Apply the YAML file using kubectl.

```sh
kubectl apply -f my-namespace.yaml
```

Create namespace for different environment

```sh
kubectl apply -f dev-namespace.yaml -f prod-namespace.yaml
kubectl apply -f deployment.yaml -n dev
kubectl apply -f deployment.yaml -n prod
```
