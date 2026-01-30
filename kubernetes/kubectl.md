# kubectl

Reference: [Install and Set Up kubectl on Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

The command-line utility to interact with the cluster.

```sh
# here I'm using a Mac Mini M1, so I will go with the Linux ARM64
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/arm64/kubectl"

# validate the binary
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check

# install kubectl
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# verify the installation
kubectl version --client
kubectl version --client --output=yaml
```

## Commands

```sh

```
