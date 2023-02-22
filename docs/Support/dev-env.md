# Dev Environment

Setup files for the development environment using all the services

0. Put env vars to `.env` (see `.env.example`)

1. Go to the `/local` folder

2. Run following command to start all services

```
docker-compose --env-file ../.env up
```

If you want to run only specific services, specify them:

```
docker-compose --env-file ../.env up postgres keycloak graph-engine
```
