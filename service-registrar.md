# Service Registrar

Service names, ports, and paths of all services and components.

## Types of Services

1. Support - services that other services need to run. ex: database, kafka.
1. Common - services that most apps depend on for basic functionality. ex: authentication, authorization.
1. Dataset - APIs that provide direct access to datasets. They may be maintained by the lab or by an org. ex: iso_639_2.
1. Org - services maintained by a specific organization for their own purposes.

## Port Convention

1. 81xx -> Support (some exceptions, notably postgres)
1. 82xx -> Common
1. 83xx -> Dataset
1. 84xx -> Org

## IP Address Convention

1. 10.1.1.x -> Support
1. 10.1.2.x -> Common
1. 10.1.3.x -> Dataset
1. 10.1.4.x -> Org

## Registration Table

| Service Name          | Service Type  | Path                  | Local Port | Docker IP   | Notes                                     |
| ---                   | ---           | ---                   | ---        | ---         | ---                                       |
| `authentication-api`  | Common        | `/api/authentication` | `8202`     | `10.1.2.2`  | register, login, logout, ...              |
| `iso_639_2-api`       | Dataset       | `/api/iso_639_2`      | `8302`     | `10.1.3.2`  | get by iso 639-2, find by English name    |
| `postgres-1`          | Support       | -                     | `5432`     | `10.1.1.2`  | postgres database                         |
| `database-api`        | Support       | `/api/database`       | `8103`     | `10.1.1.3`  | schema control for eil-db-1               |
| `pg-admin`            | Support       | -                     | `8104`     | `10.1.1.4`  | UI for local postgres database            |
| `zookeeper-0`         | Support       | -                     | `8105`     | `10.1.1.5`  | support service for local Kafka           |
| `zookeeper-1`         | Support       | -                     | `8106`     | `10.1.1.6`  | support service for local Kafka           |
| `kafka-0`             | Support       | -                     | `8107`     | `10.1.1.7`  | Kafka container                           |
| `kafka-1`             | Support       | -                     | `8108`     | `10.1.1.8`  | Kafka container                           |
| `kafka-schema-0`      | Support       | -                     | `8109`     | `10.1.1.9`  | support service for local Kafka           |
| `kafka-schema-1`      | Support       | -                     | `8110`     | `10.1.1.10` | support service for local Kafka           |
| `kafka-connect-0`     | Support       | -                     | `8111`     | `10.1.1.11` | support service for local Kafka           |
| `kafka-ui`            | Support       | -                     | `8112`     | `10.1.1.12` | UI for the local Kafka docker service     |

