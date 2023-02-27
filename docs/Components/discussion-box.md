## Description

This is a component library for rendering discussions and you can create and find discussions by `table_name` and `row`.

## Installation
```
npm install --save @eten-lab/discussion-box
```

## Usage

Set `env` vars:
```
REACT_APP_DISCUSSION_SUBSCRIPTION_WS_API=ws://localhost:8212/graphql
REACT_APP_DISCUSSION_SUBSCRIPTION_API=http://localhost:8212/graphql
REACT_APP_DISCUSSION_API=http://localhost:8202/graphql
REACT_APP_FILE_API=http://localhost:8203/graphql
REACT_APP_AGGREGATION_API=http://localhost:8206/grapqhl
REACT_APP_GRAPHQL_MODE=independant #aggregation # independant
REACT_APP_MAX_FILE_SIZE=1000000000
```

If you want to use `aggregation_api`, you can use it by modifying the env like  `REACT_APP_GRAPHQL_MODE=aggregation`

### Story 1
If you know the `userId`, `appId`, and `orgId`, you can use the `Discussion` component at the `@eten-lab/discussion-box` package.
And this is a default component for the `discussion-box`.

Required `props`:
```
export type DiscussionPureProps = {
  userId: number; // userId should be a valid id exists in users table.
  tableName: string;
  rowId: number;
  appId: number; // appId should be a valid id exists in app_list table.
  orgId: number; // orgId should be a valid id exists in organizations table.
  style?: CSSProperties | undefined;
};
```

Here is an example: 
```
import React, { useState, ChangeEventHandler } from 'react';
import { Discussion } from '@eten-lab/discussion-box';

function App() {
  return (
        <Discussion
          tableName="example"
          rowId={1}
          userId={100}
          orgId={1}
          appId={1}
          style={{
            height: 'calc(100vh - 42px)',
            padding: '20px',
            border: '1px solid #000',
          }}
        />
  );
}

export default App;

```

### Story 2

If you don't know userId, appId, and orgId, and need to create them by the user's email and names of app and org, then you can use the `DiscussionForDev` component at `@eten-lab/discussion-box`.

Required `props`:
```
type DiscussionForDevProps = {
  tableName: string;
  rowId: number;
  userEmail: string;
  orgName?: string;
  appName?: string;
  style?: CSSProperties;
};

```

Here is an example: 
```
import React, { useState, ChangeEventHandler } from 'react';
import { Discussion } from '@eten-lab/discussion-box';

function App() {
  return (
        <DiscussionForDev
          tableName="example"
          rowId={1}
          userEmail={email}
          appName="dev app"
          orgName="dev org
          style={{
            height: 'calc(100vh - 42px)',
            padding: '20px',
            border: '1px solid #000',
          }}
        />
  );
}

export default App;

```

## Note

If you want to contribute to this component, you can find `eil-discussion-box` repository in the `etenlab` account.

This component works together `discussion-api`, `discussion-subscription-api`, and `file-api`.

### Build

`npm run build`

### Running example

`npm run start:example`
