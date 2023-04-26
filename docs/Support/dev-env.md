# Dev Environment

## Setup files for the development environment using all the services

0. Put env vars to `.env` (see `.env.example`)

1. Go to the `/local` folder

2. Run following command to start all services

```sh
docker-compose --env-file ../.env up
```

If you want to run only specific services, specify them:

```sh
docker-compose --env-file ../.env up postgres keycloak graph-engine
```
Note: From the end of June 2023 Compose V1 won’t be supported anymore and will be removed from all Docker Desktop versions. `docker-compose` command will be substituted with `docker compose`.
For more information, see https://docs.docker.com/compose/.

## dump dev database from Amazon RDS

It is possible to make a dump of  the database form Amazon RDS using local docker container which is started by this docker-compose

The next command shoul be run after the postrgres container starded by mentioned docker-compose command (`local-postgres-1` here is name of the container)
``` bash
docker exec -i local-postgres-1 /bin/bash -c "pg_dump --verbose --host=dev-aurora-cluster.cluster-ro-cpxhfog2q80o.us-east-2.rds.amazonaws.com --port=5432 --username=postgres --format=plain --file /var/lib/postgresql/data/dump-eil-eildb1.sql eildb1"
```
You will be prompted to enter a password. Then all database eildb1 will be dumped.
The dump will be saved to ./local/postgres/dump-eil-eildb1.sql (you'll need a root privileges do see files at ./local/postgres)
