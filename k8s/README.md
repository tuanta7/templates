# Kubernetes & Kubeadm

Kubeadm is a tool built to provide `kubeadm init` and `kubeadm join` as best-practice fast paths for creating Kubernetes clusters

## 1. Cluster Setup

Create namespace for different environment

```sh
kubectl apply -f dev-namespace.yaml -f prod-namespace.yaml
kubectl apply -f deployment.yaml -n dev
kubectl apply -f deployment.yaml -n prod
```