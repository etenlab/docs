---
sidebar_position: 2
---

# Development Conventions

## Stack, Tools, Strategy

- Node Version: `16`
- Package Manager: `npm 8`
- Linter: [Eslint](https://eslint.org/) (see below)
- Formatter: `Prettier` (see below)
- Environment variables: every service should contain `.env.example` (even if empty) to is clear what env vars it requires to be set
- Server framework: [nestjs](https://nestjs.com/)
- API paradigm: [graphql](https://graphql.org/)
- Rest api docs (if available): `/api`
- Rest root path (`/`): HTTP 200, `{"version":"x.y.z","name":"service-name"}`
- Server graphQL library: [apollo server](https://www.apollographql.com/docs/apollo-server/)
- GraphQL endpoint: `/graphql`
- Client graphQL driver: [apollo client](https://www.apollographql.com/docs/react/)
- Frontend framework: [ionic-react](https://ionicframework.com/docs/react)
- UI Components: [mui](https://mui.com/) or [ionic](https://ionicframework.com/docs/components)
- Branching strategy:
  - `prod` branch for AWS `prod` env
  - `dev` branch for AWS `dev` env
  - `[feature-name]` feature branches for work in progress
- Deployment Targets:
  - Phase 1: Modern browsers
  - Phase 2: Android devices
  - Phase 3: Electron devices

## Eslint Config

File name: `.eslintrc.js`

File content (may be a subject of changes):

<CH.Code>

```javascript nestjs/.eslintrc.js
module.exports = {
  parser: '@typescript-eslint/parser',
  parserOptions: {
    project: 'tsconfig.json',
    tsconfigRootDir: __dirname,
    sourceType: 'module',
  },
  plugins: ['@typescript-eslint/eslint-plugin'],
  extends: [
    'plugin:@typescript-eslint/recommended',
    'plugin:prettier/recommended',
  ],
  root: true,
  env: {
    node: true,
    jest: true,
  },
  ignorePatterns: ['.eslintrc.js'],
  rules: {
    // '@typescript-eslint/interface-name-prefix': 'off',
    // '@typescript-eslint/explicit-function-return-type': 'off',
    // '@typescript-eslint/explicit-module-boundary-types': 'off',
    // '@typescript-eslint/no-explicit-any': 'off',
    semi: 2,
    // "@typescript-eslint/semi": ["error"],
    'prettier/prettier': [
      'error',
      {
        singleQuote: true,
        trailingComma: 'all',
      },
    ],
  },
};
```

```javascript reactjs/.eslintrc.js
module.exports = {
  env: {
    node: true,
    browser: true,
    es2021: true,
  },
  ignorePatterns: ['.eslintrc.js', 'config-overrides.js'],
  extends: [
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:prettier/recommended',
  ],
  overrides: [],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    project: 'tsconfig.json',
    tsconfigRootDir: __dirname,
  },
  plugins: ['react', '@typescript-eslint'],
  settings: {
    react: {
      version: 'detect',
    },
  },
  rules: {
    'react/jsx-uses-react': 'off',
    'react/react-in-jsx-scope': 'off',
    '@typescript-eslint/no-empty-interface': 'off',
    '@typescript-eslint/no-non-null-assertion': 'off',
    '@typescript-eslint/no-empty-function': 'off',
    '@typescript-eslint/dot-notation': 'error',
    '@typescript-eslint/no-unused-vars': 'off',
    'prettier/prettier': [
      'error',
      {
        trailingComma: 'all',
        tabWidth: 2,
        semi: true,
        singleQuote: true,
      },
    ],
  },
};
```

</CH.Code>

## Prettier Config

File name: `.prettierrc`

File content:

```json .prettierrc
{
  "trailingComma": "all",
  "tabWidth": 2,
  "semi": true,
  "singleQuote": true
}
```
## Logging at applications (Frontend)
### No `console.log` at final code allowed. 
If you use it during development, all `console.log` references must be deleted before merging with `dev` or `prod` branches.

### Use special logger service
It is recommended to use logger service.

### Why
1. No mess in the console at dev and prod env, yet you can tune up logs visibility in local env, because logger messages appearence is controlled by .env variables
``` sh
REACT_APP_LOGTRACEALL=FALSE
REACT_APP_LOGLEVEL=60
```
REACT_APP_LOGTRACEALL:
- if TRUE, every logger message at console will be provided with stack trace. 
- if FALSE, logger messages will be printed at console without stack trace. Default.

REACT_APP_LOGLEVEL:
only messages that with defined level (or higher) will be printed at the console.
numeric and text log levels correspondence:
- 10 trace
- 20 debug
- 30 info
- 40 warn
- 50 error
- 60 fatal, Default 

With default values (if values are not set), only fatal messages will appear at dev or prod env.

2. All logs are directed to one procession point - loggerSevice, where we can transform them or redirect or send or whatever.

### Where i cant get it
In pages/components/hooks/etc you can get logger from app context via useAppContext() hook:
``` typescript
  const {
    logger,
  } = useAppContext();
```
If services, you shoud pass loggerService as a parameter to your service' constructor at singletons.ts

example for syncService which wants to use logger:
``` typescript singletons.ts
  const syncService = new SyncService(
    loggerService,
  );
```
and then use it in your service as you wish
``` typescript syncService.ts
  constructor(
    private readonly logger: LoggerService,
  ) {}
...
  someMethod() {
    this.logger.info(`Sync!`);
  }

```

### How to use logger
logger has 6 loglevels:

logger.trace(arguments)

logger.debug(arguments)

logger.info(arguments)

logger.warn(arguments)

logger.error(arguments)

logger.fatal(arguments)

arguments:
- First argument is ether string or object.
If object, it is treated as additional user information about context.
If string, it is trated as usual logging message.

- Second and every next argument could be string or any other object which will be automatically JSON.stringified and concatenated with previous argument.



exapmle
``` typescript
  const a = 'some var';
  logger.trace({lookAt:'some information about context'}, 'some usefull message ' , a)
```
reult in console:
```
  [trace]: some usefull message some var [context]: {"logLevel":10,"lookAt":"some information about context"}
```

Remember to set .env variable `REACT_APP_LOGLEVEL` to desifred value. Unless set, default 60 is used, so you'll see only fatal messages.


## Logging at api (Backend)
Not specified yet.
