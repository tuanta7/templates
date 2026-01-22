# Failover Manager

Tools or orchestrators monitor the health of the primary node. If the primary fails, a standby is automatically promoted to primary. Examples include: Patroni, Stolon or Kubernetes Operators (e.g., CloudNativePG, CrunchyData, Zalando Postgres Operator)

## 1. Patroni

Patroni is a template for high availability (HA) PostgreSQL solutions using Python. For maximum accessibility, Patroni supports a variety of distributed configuration stores like ZooKeeper, etcd, Consul or Kubernetes.

## 2. CloudNativePG

Reference: [CloudNativePG](https://cloudnative-pg.io)
