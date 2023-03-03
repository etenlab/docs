# Add discussion to your app

## Description

This is a component library for rendering discussions and you can create and find discussions by `table_name`, `row`, `app`, and `org`.

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

If you want to use `aggregation_api`, you can use it by modifying the env like `REACT_APP_GRAPHQL_MODE=aggregation`

### Story 1

If you know the `userId`, `appId`, and `orgId`, you can use the `Discussion` component at the `@eten-lab/discussion-box` package.
This is a default component for the `discussion-box`.

- Required `props`:

```typescript
export type DiscussionPureProps = {
  userId: number; // userId should be a valid id exists in users table.
  tableName: string;
  rowId: number;
  appId: number; // appId should be a valid id exists in app_list table.
  orgId: number; // orgId should be a valid id exists in organizations table.
  height: sring; // ex: '100%', '500px', '100vh' ...
};
```

- Here is an example:

```tsx focus=6:17
import React, { useState, ChangeEventHandler } from 'react';
// mark[10:20]
import { Discussion } from '@eten-lab/discussion-box';

function App() {
  return (
    // mark[6:16]
    <Discussion
      tableName="example"
      rowId={1}
      userId={100}
      orgId={1}
      appId={1}
      height="100vh"
    />
  );
}

export default App;
```

### Story 2

If you don't know userId, appId, and orgId, and need to create them by the user's email and names of app and org, then you can use the `DiscussionForDev` component at `@eten-lab/discussion-box`.

- Required `props`:

```typescript
type DiscussionForDevProps = {
  tableName: string;
  rowId: number;
  userEmail: string;
  orgName?: string;
  appName?: string;
  height: string;
};
```

Here is an example:

```tsx focus=6:17
import React, { useState, ChangeEventHandler } from 'react';
// mark[10:26]
import { DiscussionForDev } from '@eten-lab/discussion-box';

function App() {
  return (
        // mark[10:26]
        <DiscussionForDev
          tableName="example"
          rowId={1}
          userEmail={email}
          appName="dev app"
          orgName="dev org
          height="calc(100vh-50px)"
        />
  );
}

export default App;

```
