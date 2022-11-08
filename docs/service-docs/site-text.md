# Site Text Api

## Requirements

- Node 16+
- yarn
- Docker

## Usage

### Development

- Run `yarn` to install repo dependencies.
- Run `yarn start:dev` to start the API in watch mode.

### Docker

- Run `docker-compose up`.
- Set this options in the [pg Pool](https://github.com/etenlab/site-text-api/blob/main/src/core/postgres.service.ts#L11) in order to connect to the postgres cointainer.

```
{
  user: 'postgres',
  host: 'host.docker.internal',
  database: process.env.POSTGRES_DB,
  password: process.env.POSTGRES_PASSWORD,
  port: 5433,
}
```

---

Use [Apollo Studio Sandbox](https://studio.apollographql.com/sandbox/explorer) to check the schema and all available operations.

In the Apollo Studio configuration use as endpoint the following url: `http://localhost:3001/graphql`
