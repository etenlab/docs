## Description

[Nest](https://github.com/nestjs/nest) framework TypeScript starter repository.

## Installation

```bash
$ npm install
```

## Running the app

```bash
# development
$ npm run start

# watch mode
$ npm run start:dev

# production mode
$ npm run start:prod
```

## Test

```bash
# unit tests
$ npm run test

# e2e tests
$ npm run test:e2e

# test coverage
$ npm run test:cov
```

## Setting up Subgraph

Install required dependencies

```bash
npm install --save @apollo/federation @apollo/subgraph
```

then use ApolloFederationDriverConfig and ApolloFederationDriver instead of ApolloDriverConfig and ApolloDriver

```bash
import {
  ApolloFederationDriver,
  ApolloFederationDriverConfig,
} from '@nestjs/apollo';
```

```bash
GraphQLModule.forRoot<ApolloFederationDriverConfig>({
  driver: ApolloFederationDriver,
  debug: true,
  playground: false,
  autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
  sortSchema: true,
  plugins: [ApolloServerPluginLandingPageLocalDefault()],
}),
```

## Generate Supergraph schema

Need to regenerate supergraph schema whenever we add new subgraph or make any changes in subgraph schema. We use rover cli to generate supergraph schema. see rover cli documentation [here](https://www.apollographql.com/docs/rover/)

### Add new subgraph

First need to add the subgraph in rover config ( supergraph.yaml ) as below

```bash
  voting:
    routing_url: http://localhost:8210/graphql
    schema:
      subgraph_url: http://localhost:8210/graphql
```

then need to add the subgraph endpoints to the gateway config

```bash
@Module({
  imports: [
    GraphQLModule.forRoot<ApolloGatewayDriverConfig>({
      driver: ApolloGatewayDriver,
      server: {
        // ... Apollo server options
        cors: true,
      },
      gateway: {
        supergraphSdl: new IntrospectAndCompose({
          subgraphs: [
            { name: 'users', url: 'http://user-service/graphql' },
            { name: 'posts', url: 'http://post-service/graphql' },
          ],
        }),
      },
    }),
  ],
})
```

then run rover command to generate supergraph schema (Make sure all subgraphs are up)

```bash
rover supergraph compose --config ./supergraph.yaml > src/supergraph.graphql
```

## Support

Nest is an MIT-licensed open source project. It can grow thanks to the sponsors and support by the amazing backers. If you'd like to join them, please [read more here](https://docs.nestjs.com/support).

## Stay in touch

- Author - [Kamil Myśliwiec](https://kamilmysliwiec.com)
- Website - [https://nestjs.com](https://nestjs.com/)
- Twitter - [@nestframework](https://twitter.com/nestframework)

## License

Nest is [MIT licensed](LICENSE).
