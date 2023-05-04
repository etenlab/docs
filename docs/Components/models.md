# Models

## Install

```sh
npm i @eten-lab/models
```

## Description


`Models` is a package containing all typeOrm models which are supposed to be the authoritative code on the DB schema.
Use Models in your app/api to be sure that data structure is compatible with CPG - server and can be synced.

Notes:

- long primary key names (i.e. not user.id but user.user_id are used intentionally.)

## release
``` sh
npm build
npm version patch
npm publish --access public
```