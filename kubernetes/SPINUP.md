# Kubernetes Spin-up

Homelab K8s

![OS](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=Ubuntu&logoColor=white)

## 1. K3s (Recommended)

Reference: [Quick-Start Guide](https://docs.k3s.io/quick-start)

K3s provides an installation script that is a convenient way to install it as a service on systemd or openrc based systems.

- Additional utilities will be installed including kubectl
- A single-node server installation is a fully-functional Kubernetes cluster, including all the datastore, control-plane, kubelet, and container runtime components necessary to host workload pods

```sh
curl -sfL https://get.k3s.io | sh -
```

To install additional agent nodes and add them to the cluster, run the installation script with the K3S_URL and K3S_TOKEN environment variables.

```sh
sudo cat /var/lib/rancher/k3s/server/node-token
curl -sfL https://get.k3s.io | K3S_URL=https://myserver:6443 K3S_TOKEN=mynodetoken sh -
```

## 2. Kubeadm

Reference: [Installing kubeadm v1.34](https://v1-34.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

This section contains kubeadm-based installation steps for control-plane and worker nodes using `kubeadm`

> [!NOTE]
> Docker Engine depends on containerd and runc. Docker Engine bundles these dependencies as one bundle: containerd.io

### 2.1. Control Plane

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

Calico

Reference: [Install Calico networking and network policy for on-premises deployments](https://docs.tigera.io/calico/latest/getting-started/kubernetes/self-managed-onprem/onpremises)

```sh
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

### 2.2. Worker Nodes

Reference: [Adding Linux worker nodes](https://v1-34.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/adding-linux-nodes/)

Each joining worker node must have `kubeadm`, `kubelet`, and a container runtime installed.

- `kubelet` runs on all machines in the cluster and is responsible for starting pods and containers.

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
