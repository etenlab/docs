# File API

## Description

Supports file upload apis. Whenever get a file upload request, upload it to AWS s3 bucket and then store the details in a file table.

## Installation

`npm install`

## Running the app

Set `env` vars:

```
DB_HOST=127.0.0.1
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=asdfasdf
DB_NAME=eil_db_1
AWS_S3_REGION=
AWS_S3_BUCKET_NAME=
AWS_S3_ACCESS_ID=
AWS_S3_SECRET_KEY=
MAX_FILE_SIZE= 1073741824 # 1024 * 1024 * 1024
MAX_FILES=10
PORT=8203

```

`npm run start`

# watch mode

`npm run start:dev`

# production mode

`npm run start:prod`

## Test

`npm run test`

# e2e tests

`npm run test:e2e`

# test coverage

`npm run test:cov`
