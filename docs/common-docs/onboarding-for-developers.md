# Onboarding for Developers

This document aims quick and flawless onboarding for a developer.

## Deploy all services locally with docker-compose

1. Make sure you have `docker` installed in your system
2. Create a folder where you will have all the project's repos; go to it and open terminal
3. Clone `dev-env` repo:

```bash
git clone git@github.com:etenlab/dev-env.git
```

4. Great, now come in: `cd ./dev-env`
5. Configure `.env` file using `.env.example`:
6. Make sure to have the latest images: run `docker-compose pull`. Use this command to keep your services up-to-date.
7. Run

```bash
docker-compose -f local/docker-compose.yml --env-file .env up
```

Some services may fail due to lack of data at this moment. Now, let's fill the database with some data

Notes:

- If some of the services fail to listen on specific ports, ensure the exposed ports in you `.env` file not to be taken by other processes in your host system (listening on `localhost`). To check that, run `sudo netstat -tlnp` (Ubuntu) or other command specific to your OS.
- If you want to run only specific services, provide their names at the end of the command, e.g. `docker-compose -f local/docker-compose.yml --env-file .env up database-api discussion-api`
- If for some reason you run `docker` outside your docker's host system, e.g. in a devcontainer, virtual machine, etc, make sure to have correct value for variable `KC_DB_INIT_SCRIPT_PATH` as it is should point to the file on your docker's host system.

## Fill the database with data

1. Clone `datasets` repo:

```bash
git clone git@github.com:etenlab/datasets.git
```

2. Clone `scripts` repo:

```bash
git clone git@github.com:etenlab/scripts.git
```

3. Configure database parameters in `./scripts/dataset-scripts/eilcommon.py`
4. Download necessary python modules:

```bash
pip install psycopg2-binary bs4
```

5. Run the script:

```bash
sh scripts/dataset-scripts/gimme.sh
```

Ensure no errors in the output.

6. After running the script, open `localhost:${PORT_GRAPHQL}` (Hasura web interface) and ensure your data is tracked. If not tracked, add it manually using url like this: `postgresql://postgres:example@postgres_db:5432/eil` (`postgresql://${DB_USERNAME}:${DB_PASSWORD}@postgres_db:5432/${DB_EIL_DATABASE}`). Set schema name to `public`.

**Congratulations**! Now the database is filled with data.

## Develop in the local environment

Now that you have setup your local environment, let's consider two ways of working with that.

### Development on `localhost`

You can develop both frontend and backend services configuring their `.env` to interact with the services in the docker envirenment via `localhost:{SERVICE_PORT}`

### Containerized development

If you prefer to develop application in containern (e.g. VSCode [devcontainer](https://code.visualstudio.com/docs/devcontainers/containers)) to keep your host system clean of packages / NodeJS versions / extensions etc, this approach is much for you.

1. Get the name of the environment's network. It can be found in the compose config file, with `docker network ls` or with `docker inspect {container ID from the environment`}.

2. Attach your container to the docker network of the environment. To do this, your container should be run with parameter `--network={network name}`. For devcontainers, you will have the next config on `./.devcontainer/devcontainer.json`:

```json
"runArgs": [
    "--network=eil" // here `eil` the name of the network
    , "--name=database-api-devcontainer" // optionally, set the name for this container so it is availabe for services in docker-compose
],
```
