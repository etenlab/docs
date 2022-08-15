# Service Registrar

Service names, ports, and paths of all services and components.

| Service Name | Path | Local Port | Notes |
| ---- | ---- | ---- | --- |
| `authentication` | `/api/authentication` | `8090` | register, login, logout, ... |
| `iso_639_2` | `/api/iso_639_2` | `8091` | get by iso 639-2, find by English name |
| `kafka-ui` | - | `8080` | UI for the local Kafka docker service |
| `kafka-connect-1` | - | `8083` | support service for local Kafka |
| `kafka-schema-1` | - | `8085` | support service for local Kafka |
| `kafka-schema-2` | - | `18085` | support service for local Kafka |
| `kafka-1` | - | `9092`, `9997` | Kafka container |
| `kafka-2` | - | `9093`, `9998` | Kafka container |
| `pg-admin` | - | `5050` | UI for local postgres database |
| `zookeeper-1` | - | `2181` | support service for local Kafka |
| `zookeeper-2` | - | - | support service for local Kafka |
| `postgres-1` | - | `5432` | postgres database |

