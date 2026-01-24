# Ubuntu Server Setup

![OS](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=Ubuntu&logoColor=white)

## Docker

Reference: [Install Docker Engine](https://docs.docker.com/engine/install/)

Using the `apt` repository

```sh
# uninstall all conflicting packages
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)

# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update

# install the latest version
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# verify the installation
sudo docker run hello-world
```

Notes: The Docker daemon binds to a Unix socket, not a TCP port. By default the socket is owned by root; add users to the `docker` group to allow non-root use.

```sh
# create the docker group
sudo groupadd docker

# add user to the docker group.
sudo usermod -aG docker $USER

# create new user if needed
sudo adduser newuser
```

## Kubernetes

This section contains kubeadm-based installation steps for control-plane and worker nodes. Ensure versions of `kubeadm`, `kubelet`, and `kubectl` are compatible.

### Control Plane

Installing `kubeadm`, `kubelet` and `kubectl`.

Reference: [Installing kubeadm v1.34](https://v1-34.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

```sh
# update the apt package index and install needed packages
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gpg

# download the public signing key for the Kubernetes package repositories
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.34/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# add the appropriate Kubernetes apt repository
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.34/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

# update the apt package index, install kubelet, kubeadm and kubectl
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl

# pin their version
sudo apt-mark hold kubelet kubeadm kubectl

# enable the kubelet service before running kubeadm
sudo systemctl enable --now kubelet
```

Create a minimum viable cluster with `kubeadm`.

Reference: [Creating a cluster with kubeadm](https://v1-34.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)

```sh
kubeadm init
kubeadm init --control-plane-endpoint "LOAD_BALANCER_DNS:LOAD_BALANCER_PORT" --upload-certs

mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

If containerd config needs reset:

```sh
sudo rm /etc/containerd/config.toml
sudo systemctl restart containerd
```

### Calico

```

```

### Worker Node

Reference: [Adding Linux worker nodes](https://v1-34.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/adding-linux-nodes/)

Each joining worker node must have `kubeadm`, `kubelet`, and a container runtime installed.

```sh
sudo kubeadm join --token <token> <control-plane-host>:<control-plane-port> --discovery-token-ca-cert-hash sha256:<hash>
```

Commands to retrieve or create tokens on the control plane:

```sh
# get the value for token
sudo kubeadm token list

# create a new token if needed
sudo kubeadm token create
sudo kubeadm token create --print-join-command

# get the value for discovery-token-ca-cert-hash
sudo cat /etc/kubernetes/pki/ca.crt | openssl x509 -pubkey  | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'
```

#### kubectl

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

#### kubelet

The `kubelet` runs on all machines in the cluster and is responsible for starting pods and containers.

### Appendix: Minikube

Local single-node Kubernetes for development/testing.

```

```

## Coding

Developer tooling installation snippets.

### Go

```shell
# go to home directory
cd ~

# install Go
wget https://go.dev/dl/go1.25.0.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.25.0.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
source ~/.bashrc
go version

# back to the previous directory
cd -
```

### NodeJS

```shell
# install nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # This loads nvm bash_completion

source ~/.bashrc
nvm --version

# install node
nvm install --lts
```

```

```
