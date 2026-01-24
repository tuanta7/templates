# Kubernetes

This file defines an approach for setting up a Docker and Kubernetes cluster with Argo CD, Helm, and Rancher, along with a highly available PostgreSQL deployment using the CloudNativePG operator. Additional automation is subsequently applied using Ansible.

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

## 2. Custom Resources

A resource is an endpoint in the Kubernetes API that stores a collection of API objects of a certain kind; for example, the built-in pods resource contains a collection of Pod objects.

Custom resources are extensions of the Kubernetes API.

### Custom Controllers

### Operator Pattern

Operators are software extensions to Kubernetes that make use of custom resources to manage applications and their components
