## Data-Load API

Data-Load API is used to load data into the main database. Exposes REST endpoints.

## Authorization

To pass the authorization, one must provide header `Authorization: Basic BASE64_ENCODED(login:password)`.
The credentials are stored in env var `BASIC_AUTH_CREDENTIALS`.

## Endpoints

### POST /strong/create

Use this endpoint only once to create Strong's dictionary in the database. If one already exists, it will be re-created, so all related nodes will loose their connections to Strong's Words.

### POST /scripture/by-url

Body:

```json
{
  "url": "string"
}
```

Receives a url to the USFM text containing a Scripture.
Returns a list of created books' node ids.
