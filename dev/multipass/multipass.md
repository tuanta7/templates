# Multipass Setup 

Reference: [Manage instances](https://documentation.ubuntu.com/multipass/latest/how-to-guides/manage-instances/)

This document describes the initialization of a lightweight Ubuntu Server environment using Multipass on a macOS-based Mac mini. 

The resulting instance is intended to operate as a headless server, accessed exclusively via SSH. Network connectivity is established through a bridged Wi-Fi interface with a manually assigned (static) IP address to ensure consistent reachability on the local network.

## Instance Lifecycle Management (Clean State)

The following commands are used to inspect, remove, and permanently purge Multipass instances in order to maintain a clean and reproducible server environment.

```sh
# List all existing instances
multipass list

# Mark a specific instance as deleted (recoverable state)
multipass delete <instance-name>

# Permanently delete a specific instance in a single step
multipass delete --purge <instance-name>

# Delete all instances
multipass delete --all
```

## Ubuntu Server Launch with Bridged Networking

A bridged network configuration is used so that the Ubuntu Server instance appears as a first-class device on the local network. This enables direct SSH access using a fixed IP address assigned at the network level.

```sh
# Display available network interfaces
multipass networks

# Inspect the currently configured bridged network
multipass get local.bridged-network

# Configure the bridged network to use the Wi-Fi interface (example: en1)
multipass set local.bridged-network=en1

# Launch an Ubuntu Server instance with bridged networking enabled
multipass launch --name server --bridged

# Access server instance
multipass shell server
```

## Next Steps

- Install Docker: [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- Install Kubectl (ARM64): [Install kubectl on Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)