<p align="center">
  <img src="assets/banner.svg" alt="whoami: Kiryl Butar, Go backend developer" width="100%"/>
</p>

<p align="center">
  <a href="https://www.linkedin.com/in/kiribu/"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=data%3Aimage%2Fsvg%2Bxml%3Bbase64%2CPHN2ZyBmaWxsPSJ3aGl0ZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2aWV3Qm94PSIwIDAgMTI4IDEyOCI%2BPHBhdGggZD0iTTExNiAzSDEyYTguOTEgOC45MSAwIDAwLTkgOC44djEwNC40MmE4LjkxIDguOTEgMCAwMDkgOC43OGgxMDRhOC45MyA4LjkzIDAgMDA5LTguODFWMTEuNzdBOC45MyA4LjkzIDAgMDAxMTYgM3pNMzkuMTcgMTA3SDIxLjA2VjQ4LjczaDE4LjExem0tOS02Ni4yMWExMC41IDEwLjUgMCAxMTEwLjQ5LTEwLjUgMTAuNSAxMC41IDAgMDEtMTAuNTQgMTAuNDh6TTEwNyAxMDdIODguODlWNzguNjVjMC02Ljc1LS4xMi0xNS40NC05LjQxLTE1LjQ0cy0xMC44NyA3LjM2LTEwLjg3IDE1VjEwN0g1MC41M1Y0OC43M2gxNy4zNnY4aC4yNGMyLjQyLTQuNTggOC4zMi05LjQxIDE3LjEzLTkuNDFDMTAzLjYgNDcuMjggMTA3IDU5LjM1IDEwNyA3NXoiLz48L3N2Zz4%3D" alt="LinkedIn"/></a>
  <a href="https://t.me/kiryldev"><img src="https://img.shields.io/badge/Telegram-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram"/></a>
  <a href="mailto:kiryshabutor@gmail.com"><img src="https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white" alt="Email"/></a>
</p>

<p align="center">
  <b>Go backend developer who cares where every event ends up.</b><br/>
  Intern at Innowise · 3rd year at BSUIR · Minsk · English B2
</p>

<p align="center">
  <img src="assets/highlights.svg" alt="Highlights" width="100%"/>
</p>

## 🧭 What I do

<table>
<tr>
<td width="33%" valign="top">

### 📨 Event-driven backends
Go microservices that talk over gRPC and Kafka.
Transactional Outbox, idempotent consumers, retry with backoff, DLQ.
No lost events, no duplicates.

</td>
<td width="33%" valign="top">

### ☸️ Kubernetes platform
Operators on Kubebuilder and controller-runtime, mutating and validating webhooks.
Self-service environments on Crossplane v2.

</td>
<td width="33%" valign="top">

### 🔬 Proof, not hope
Unit, integration and E2E tests.
Prometheus metrics, OpenTelemetry traces in Jaeger, load and chaos tests before I call it done.

</td>
</tr>
</table>

## 🚀 Featured work

<table>
<tr>
<td width="50%" valign="top">

#### ☸️ [jobpool-operator](https://github.com/kiryshabutor/jobpool-operator)
Keeps batch Jobs from flooding a namespace. A mutating webhook queues the extras, the controller releases them FIFO. A validating webhook with an optimistic lock lets exactly one JobPool per namespace through.

`500 Jobs → max 10 at once` · `89% coverage`

<sub>Go · Kubebuilder · controller-runtime · cert-manager · kind</sub>

</td>
<td width="50%" valign="top">

#### 🏗️ [workspace-factory](https://github.com/kiryshabutor/workspace-factory)
One YAML gives a team its environment. An XRD with CEL validation and a Pipeline Composition create namespace, ResourceQuota and Deployment in the fleet cluster. Manual edits are rolled back.

`1 YAML → full workspace` · `drift auto-healed`

<sub>Crossplane v2 · CEL · Kubernetes · kind</sub>

</td>
</tr>
<tr>
<td width="50%" valign="top">

#### 🚕 RideCore
Study taxi platform, built solo: User, Auth, Driver, Order, Wallet, Analytics and API Gateway. Events flow through Transactional Outbox and survive SIGKILL and a 30 s Kafka outage.

`7 services` · `~3 100 RPS, p99 < 70 ms` · `83% coverage`

<sub>Go · gRPC · Kafka · PostgreSQL · MongoDB · Redis · ClickHouse · OpenTelemetry</sub>

</td>
<td width="50%" valign="top">

#### ⏰ [event-driven-reminders](https://github.com/kiryshabutor/event-driven-reminders)
Delayed reminders across five services with a REST gateway, internal gRPC and access/refresh tokens. Kafka events go out through Transactional Outbox and are deduplicated by EventID in the same SQL transaction.

`5 services` · `dedup by EventID`

<sub>Go · PostgreSQL · Redis · Kafka · gRPC</sub>

</td>
</tr>
</table>

<p align="center">
  <sub>Smaller things: <a href="https://github.com/kiryshabutor/load-balancer">load-balancer</a> (lock-free round-robin, health checks) · <a href="https://github.com/kiryshabutor/lru-cache-proxy">lru-cache-proxy</a> (thread-safe O(1) LRU)</sub>
</p>

## 🔁 How an event travels in my systems

```mermaid
flowchart LR
    U([Client]) -->|REST + JWT| GW[API Gateway]
    GW -->|gRPC| SVC[Service]
    SVC -->|state + outbox row,<br/>one transaction| PG[(PostgreSQL)]
    PG -->|relay| K[(Kafka)]
    K --> C[Idempotent consumer]
    C -->|retry with backoff| C
    C -.->|gave up| DLQ[(DLQ)]
    C --> OUT[(ClickHouse · MongoDB · Redis)]

    classDef go fill:#00add8,stroke:#007d9c,color:#fff;
    class GW,SVC,C go;
```

## 🧰 Stack

<table>
<tr><td><b>Core</b></td><td align="center" width="96"><img src="assets/icons/go.svg" width="40" height="40" alt="Go"/><br/><sub>Go</sub></td><td align="center" width="96"><img src="assets/icons/grpc.svg" width="40" height="40" alt="gRPC · Protobuf"/><br/><sub>gRPC · Protobuf</sub></td><td align="center" width="96"><img src="assets/icons/swagger.svg" width="40" height="40" alt="OpenAPI"/><br/><sub>OpenAPI</sub></td><td align="center" width="96"><img src="assets/icons/jwt.svg" width="40" height="40" alt="JWT"/><br/><sub>JWT</sub></td></tr>
<tr><td><b>Data</b></td><td align="center" width="96"><img src="assets/icons/postgresql.svg" width="40" height="40" alt="PostgreSQL"/><br/><sub>PostgreSQL</sub></td><td align="center" width="96"><img src="assets/icons/mongodb.svg" width="40" height="40" alt="MongoDB"/><br/><sub>MongoDB</sub></td><td align="center" width="96"><img src="assets/icons/redis.svg" width="40" height="40" alt="Redis"/><br/><sub>Redis</sub></td><td align="center" width="96"><img src="assets/icons/elasticsearch.svg" width="40" height="40" alt="Elasticsearch"/><br/><sub>Elasticsearch</sub></td><td align="center" width="96"><img src="assets/icons/clickhouse.svg" width="40" height="40" alt="ClickHouse"/><br/><sub>ClickHouse</sub></td></tr>
<tr><td><b>Messaging</b></td><td align="center" width="96"><img src="assets/icons/kafka.svg" width="40" height="40" alt="Kafka"/><br/><sub>Kafka</sub></td><td align="center" width="96"><img src="assets/icons/rabbitmq.svg" width="40" height="40" alt="RabbitMQ"/><br/><sub>RabbitMQ</sub></td></tr>
<tr><td><b>Platform</b></td><td align="center" width="96"><img src="assets/icons/kubernetes.svg" width="40" height="40" alt="Kubernetes · Kubebuilder"/><br/><sub>Kubernetes · Kubebuilder</sub></td><td align="center" width="96"><img src="assets/icons/crossplane.svg" width="40" height="40" alt="Crossplane"/><br/><sub>Crossplane</sub></td><td align="center" width="96"><img src="assets/icons/cert-manager.svg" width="40" height="40" alt="cert-manager"/><br/><sub>cert-manager</sub></td><td align="center" width="96"><img src="assets/icons/docker.svg" width="40" height="40" alt="Docker"/><br/><sub>Docker</sub></td><td align="center" width="96"><img src="assets/icons/aws.svg" width="56" height="40" alt="S3 · EKS · IAM"/><br/><sub>S3 · EKS · IAM</sub></td></tr>
<tr><td><b>Delivery</b></td><td align="center" width="96"><img src="assets/icons/gitlab.svg" width="40" height="40" alt="GitLab CI/CD"/><br/><sub>GitLab CI/CD</sub></td><td align="center" width="96"><img src="assets/icons/githubactions.svg" width="40" height="40" alt="GitHub Actions"/><br/><sub>GitHub Actions</sub></td><td align="center" width="96"><img src="assets/icons/git.svg" width="40" height="40" alt="Git"/><br/><sub>Git</sub></td><td align="center" width="96"><img src="assets/icons/linux.svg" width="40" height="40" alt="Linux"/><br/><sub>Linux</sub></td></tr>
<tr><td><b>Observe</b></td><td align="center" width="96"><img src="assets/icons/prometheus.svg" width="40" height="40" alt="Prometheus"/><br/><sub>Prometheus</sub></td><td align="center" width="96"><img src="assets/icons/grafana.svg" width="40" height="40" alt="Grafana"/><br/><sub>Grafana</sub></td><td align="center" width="96"><img src="assets/icons/opentelemetry.svg" width="40" height="40" alt="OpenTelemetry"/><br/><sub>OpenTelemetry</sub></td><td align="center" width="96"><img src="assets/icons/jaeger.svg" width="40" height="40" alt="Jaeger"/><br/><sub>Jaeger</sub></td></tr>
<tr><td><b>Daily tools</b></td><td align="center" width="96"><img src="assets/icons/postman.svg" width="40" height="40" alt="Postman"/><br/><sub>Postman</sub></td><td align="center" width="96"><img src="assets/icons/claude.svg" width="40" height="40" alt="Claude Code"/><br/><sub>Claude Code</sub></td><td align="center" width="96"><img src="assets/icons/cursor.svg" width="40" height="40" alt="Cursor"/><br/><sub>Cursor</sub></td></tr>
</table>

---

<p align="center">
  <sub>Currently: Go Backend Developer Intern @ Innowise · Looking forward to: highload</sub>
</p>
