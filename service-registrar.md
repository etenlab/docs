# Service Registrar

Service names, ports, and paths of all services and components.

| Service Name | Path | Local Port | Docker IP | Notes |
| --- | --- | --- | --- | --- |
| `authentication` | `/api/authentication` | `8090` | `10.1.0.13` | register, login, logout, ... |
| `iso_639_2` | `/api/iso_639_2` | `8091` | `10.1.0.12` | get by iso 639-2, find by English name |
| `postgres-1` | - | `5432` | `10.1.0.2` | postgres database |
| `pg-admin` | - | `5050` | `10.1.0.3` | UI for local postgres database |
| `kafka-ui` | - | `8080` | `10.1.0.4` | UI for the local Kafka docker service |
| `kafka-0` | - | `9092`, `9997` | `10.1.0.6` | Kafka container |
| `kafka-1` | - | `9093`, `9998` | `10.1.0.8` | Kafka container |
| `kafka-connect-0` | - | `8083` | `10.1.0.11` | support service for local Kafka |
| `kafka-schema-0` | - | `8085` | `10.1.0.9` | support service for local Kafka |
| `kafka-schema-1` | - | `18085` | `10.1.0.10` | support service for local Kafka |
| `zookeeper-0` | - | `2181` | `10.1.0.5` | support service for local Kafka |
| `zookeeper-1` | - | - | `10.1.0.7` | support service for local Kafka |

