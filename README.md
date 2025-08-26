# Environment Setup (Ubuntu)

## 1. Go

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

## 2. NodeJS

```shell
# install nvm

# install node.js
```

## 3. Databases & Queue

## 4. Monitoring

### 4.1. OTel Collector

### 4.2. Jaeger + Elasticsearch

### 4.3. Prometheus

### 4.4. Grafana

### 4.5. SigNoz

## 5. OAuth