## Install Docker

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
