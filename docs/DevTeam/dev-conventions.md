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
