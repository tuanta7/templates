# ArgoCD

Reference: [ArgoCD Docs](https://argo-cd.readthedocs.io/en/stable/getting_started/)

In most deployments, Argo CD is installed only once, usually in the `argocd` namespace, while applications are deployed into differents environments

```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# grant access to other namespaces
kubectl label namespace dev  argocd.argoproj.io/managed-by=argocd
kubectl label namespace prod argocd.argoproj.io/managed-by=argocd

# get default admin password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```