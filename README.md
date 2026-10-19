<p align="center">
  <img src="assets/banner.svg" alt="whoami: Kiryl Butar, Go backend developer" width="100%"/>
</p>

```go
package main

import "fmt"

type Stack struct {
	Language  string
	Backend   []string
	Data      []string
	Infra     []string
	Quality   []string
}

type Developer struct {
	Name   string
	Role   string
	School string
	City   string
	Stack  Stack
	Goal   string
}

func main() {
	me := Developer{
		Name:   "Kiryl Butar",
		Role:   "Go Backend Developer Intern @ Innowise",
		School: "BSUIR, Computer Engineering, 3rd year",
		City:   "Minsk",
		Stack: Stack{
			Language: "Go",
			Backend:  []string{"REST", "gRPC", "Protobuf", "OpenAPI/Swagger", "JWT", "Microservices", "Clean Architecture"},
			Data:     []string{"PostgreSQL", "MongoDB", "Redis", "Elasticsearch", "ClickHouse", "Kafka", "RabbitMQ"},
			Infra:    []string{"Docker", "Kubernetes", "Kubebuilder", "Crossplane", "AWS S3/EKS/IAM/Secrets Manager", "GitLab CI/CD", "GitHub Actions"},
			Quality:  []string{"Testify", "GoMock", "Mockery", "Prometheus", "Grafana", "OpenTelemetry", "Jaeger"},
		},
		Goal: "highload",
	}

	for {
		fmt.Println("outbox -> kafka -> idempotent consumer, no duplicates, no losses:", me.Goal)
		// TODO: sleep
	}
}
```

## How I like to build things

```mermaid
flowchart LR
    U([Client]) -->|REST + JWT| GW[API Gateway<br/>Go]
    GW -->|gRPC + protobuf| SVC[Domain services<br/>Go]
    SVC -->|one SQL transaction:<br/>state + outbox row| PG[(PostgreSQL)]
    PG -->|outbox relay| K[(Kafka)]
    K -->|dedup by EventID| C[Idempotent consumers<br/>retry + backoff]
    C -.->|poison messages| DLQ[(DLQ)]
    C --> AN[(ClickHouse · MongoDB)]
    SVC <-->|cache| R[(Redis)]
    SVC -.->|metrics, traces| OBS[Prometheus · Grafana<br/>OpenTelemetry · Jaeger]
    OP{{Kubernetes operator<br/>reconcile loop}} -.->|observe, diff, act| SVC

    classDef go fill:#00add8,stroke:#007d9c,color:#fff;
    class GW,SVC,C,OP go;
```

Events that are never lost and never doubled: Transactional Outbox, idempotent consumers, retry with backoff, DLQ. Proven with a chaos test, not just claimed: services killed with SIGKILL, Kafka down for 30 s, 1000 of 1000 registrations accepted, 0 lost, 0 duplicates.

## `kubectl get projects`

| NAME | STACK | WHAT IT DOES |
|---|---|---|
| [jobpool-operator](https://github.com/kiryshabutor/jobpool-operator) | Go · Kubebuilder · controller-runtime · webhooks · cert-manager | Keeps batch Jobs from overloading a namespace: a mutating webhook queues the extras, the controller runs them FIFO. A validating webhook with an optimistic lock allows exactly one JobPool per namespace |
| [workspace-factory](https://github.com/kiryshabutor/workspace-factory) | Crossplane v2 · CEL · kind | One YAML creates a tenant workspace in the fleet cluster: namespace, ResourceQuota, Deployment. Manual drift is rolled back |
| RideCore | Go · gRPC · Kafka · PostgreSQL · MongoDB · Redis · ClickHouse · OpenTelemetry | Study taxi platform: 7 microservices, 83% test coverage, about 3 100 RPS at p99 under 70 ms through the gateway |
| [event-driven-reminders](https://github.com/kiryshabutor/event-driven-reminders) | Go · PostgreSQL · Redis · Kafka · gRPC | Delayed reminders across 5 services with Transactional Outbox and dedup by EventID |
| [load-balancer](https://github.com/kiryshabutor/load-balancer) | Go · sync/atomic | HTTP balancer with lock-free round-robin and health checks |
| [lru-cache-proxy](https://github.com/kiryshabutor/lru-cache-proxy) | Go | HTTP proxy with a thread-safe LRU cache, O(1) |

## Toolbox

**Language**

<img src="https://skillicons.dev/icons?i=go&theme=dark" alt="Go"/>

**Data and messaging**

<img src="https://skillicons.dev/icons?i=postgres,mongodb,redis,elasticsearch,kafka,rabbitmq&theme=dark" alt="PostgreSQL, MongoDB, Redis, Elasticsearch, Kafka, RabbitMQ"/>

ClickHouse

**Infrastructure**

<img src="https://skillicons.dev/icons?i=docker,kubernetes,aws,gitlab,githubactions&theme=dark" alt="Docker, Kubernetes, AWS, GitLab, GitHub Actions"/>

Kubebuilder · Crossplane · AWS (S3, CloudFront, EKS, IAM, Secrets Manager)

**Testing and monitoring**

<img src="https://skillicons.dev/icons?i=prometheus,grafana&theme=dark" alt="Prometheus, Grafana"/>

Testify · GoMock · Mockery · OpenTelemetry · Jaeger

**Tools**

<img src="https://skillicons.dev/icons?i=git,linux,postman&theme=dark" alt="Git, Linux, Postman"/>

Claude Code · Codex · Cursor

---

<p align="center">
  <sub>Telegram: <a href="https://t.me/kiryldev">@kiryldev</a> · English B2 · open to highload backend roles</sub>
</p>
