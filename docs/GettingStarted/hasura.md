# Preparing Hasura

For running Hasura server in production, some manual actions are needed.

## Service setup

Ensure you have set env vars for the Docker image:

```
- HASURA_GRAPHQL_ENABLE_CONSOLE: 'true'
- HASURA_GRAPHQL_DEV_MODE: 'false'
- HASURA_GRAPHQL_ADMIN_SECRET: '********' # Use this secret to login into Console
- HASURA_GRAPHQL_UNAUTHORIZED_ROLE: anonymous
```

## Configuring Connection to Database

The step of configuring the connection is described in [Onboardong](./onboarding-for-developers.md).

## Setting permission

Running Hasura in a public network requires setting admin secret and specifying user permission.

For each relation in the list, go to Permissions tab and grant `Select` permissions to user `anonymous`.
