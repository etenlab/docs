---
sidebar_position: 2
---

# Development Conventions

- Node Version: `16`
- Package Manager: `npm 8`
- Linter: `Eslint` ([Config](./config/eslint_config.md))
- Formatter: `Prettier` ([Config](./config/prettier_config.md))
- Server framework: `nestjs`
- API paradigm: `graphql`
- Rest api docs (if available): `/api`
- Rest root path (`/`): HTTP 200, `{"version":"x.y.z","name":"service-name"}`
- Server graphQL library: `apollo`
- GraphQL endpoint: `/graphql`
- Client graphQL driver: `apollo`
- Frontend framework: `ionic-react`
- Branching strategy:
  - `prod` branch for AWS `prod` env
  - `dev` branch for AWS `dev` env
  - `[feature-name]` feature branches for work in progress
