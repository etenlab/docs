---
sidebar_position: 2
---

# Development Conventions

## Stack, Tools, Strategy

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
- Deployment Targets:
  - Phase 1: Modern browsers
  - Phase 2: Android devices
  - Phase 3: Electron devices

## Eslint Config

File name: `.eslintrc.js`

File content (may be a subject of changes):

```
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
        "semi": 2,
        // "@typescript-eslint/semi": ["error"],
        'prettier/prettier': [
            'error',
            {
                singleQuote: true,
                trailingComma: 'all',
            }
        ],
    },
};
```

## Prettier Config

File name: `.prettierrc`

File content:

```
{
  "singleQuote": true,
  "trailingComma": "all"
}
```
