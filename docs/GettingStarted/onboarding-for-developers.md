---
sidebar_position: 1
---

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

Some services may temporary fail, but eventually there should be no errors in the console. At this point, everything is running correctly. Now, let's fill the database with some data

Notes:

- If some of the services fail to listen on specific ports, ensure the exposed ports in you `.env` file not to be taken by other processes in your host system (listening on `localhost`). To check that, run `sudo netstat -tlnp` (Ubuntu) or other command specific to your OS.
- If you want to run only specific services, provide their names at the end of the command, e.g. `docker-compose -f local/docker-compose.yml --env-file .env up database-api discussion-api`
- If for some reason you run `docker` outside your docker's host system, e.g. in a devcontainer, virtual machine, etc, make sure to have correct value for variable `KC_DB_INIT_SCRIPT_PATH` as it is should point to the file on your docker's host system.

## Setup development in VS Code

We are using eslint and prettier in our repos. To run the linter and fix errors:

```bash
npm run lint
npm run format
```

```json .vscode/settings.json
{
	"editor.codeActionsOnSave": {
		"source.fixAll.eslint": true
	},
	"eslint.enable": true,
	"eslint.format.enable": true,
	"editor.formatOnSave": true
}
```

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
