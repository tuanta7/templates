# High Availability PostgreSQL on K8s

A High Availability PostgreSQL database refers to a deployment pattern of PostgreSQL that is designed to remain continuously accessible, even in the presence of hardware failures, crashes, or network issues.

## 1. Replication

Multiple database instances (primary and replicas) are maintained, so if one fails, another can take over.

- **Streaming replication**: PostgreSQL database supports replication by streaming Write-Ahead Log (WAL) records from the primary to standby servers in real time.
- **Synchronous replication**: Guarantees zero data loss by waiting for at least one confirmation from a replica/standby before acknowledging a commit.
- **Asynchronous replication**: No confirmation, faster but may risk small data loss if the primary fails suddenly.

Read-only traffic can be distributed across standby replicas, improving performance while the primary handles write operations.

## 2. HA Architecture (Typically)

The main objective is to minimize downtime and data loss while ensuring that applications relying on the database can continue operating without significant interruption.

- **Primary Node**: Handles all writes and can serve reads.
- **Standby Nodes** (replicas): Continuously replicate data from the primary; can serve read-only queries.
- **Failover Manager**: Detects failures, promotes a standby to primary, and redirects traffic.
- **VIP** (Virtual IP) or **Load Balancer**: Provides a stable connection endpoint that always points to the current primary.

> [!NOTE]
> To avoid split-brain scenarios (where two nodes think they are primary), HA setups typically use consensus systems like etcd, Consul, or ZooKeeper.

### Failover Manager

Tools or orchestrators monitor the health of the primary node. If the primary fails, a standby is automatically promoted to primary. Examples include: Patroni, Stolon or Kubernetes Operators (e.g., CloudNativePG, CrunchyData, Zalando Postgres Operator)

## 3. CloudNativePG

Reference: [CloudNativePG Quickstart](https://cloudnative-pg.io/documentation/1.27/quickstart/)

[CloudNativePG](https://github.com/cloudnative-pg/cloudnative-pg) is a Kubernetes operator that covers the full lifecycle of a highly available PostgreSQL database cluster with a primary/standby architecture.

```sh
#
kubectl apply --server-side -f https://raw.githubusercontent.com/cloudnative-pg/cloudnative-pg/release-1.27/releases/cnpg-1.27.0.yaml

#
kubectl rollout status deployment -n cnpg-system cnpg-controller-manager
```
