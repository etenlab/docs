# Conventions

## Database Procedures

- procedure/function names:
  - snake_case
  - pattern: `[service_name]_[function_name]`
  - example: `authentication_register()`

## Code Formatting

Format code with `prettier` using the next config (`.prettierrc`):

```json
{
  "singleQuote": true,
  "trailingComma": "all"
}
```

Files to be formatted with prettier: js, jsx, ts, tsx, json, md
