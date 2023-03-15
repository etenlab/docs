# Voting Component

## Installation

```
npm install --save @eten-lab/voting
```

## Usage

Set `env` var in your app:
```
REACT_APP_VOTING_API=http://localhost:9990/graphql
```

### withVoting

Use this higher order component to add voting to your app. 
You need  call the `withVoting` function passing as parameters the election and ballot entries components

```tsx
const Voting = withVoting(ElectionComponent, [BallotEntry1, BallotEntry2]); 
``` 

Required props for the `Voting` component:

```tsx
interface VotingProps {
  electionProps: {
    appId: number;
    tableName: string;
    row: number;
    name: string;
    displayElection?: boolean;
  };
  ballotEntriesProps: {
    tableName: string;
    row: number;
  }[];
  direction: "row" | "column-reverse";
  userId?: string;
}
```

Example:

```tsx
import { withVoting } from "@eten-lab/voting";

function App() {
  const Election = () => <h1>Election</h1>;
  const BallotEntry1 = () => (
    <p>
      1. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
      tempor incididunt ut labore et dolore magna aliqua.
    </p>
  );

  const BallotEntry2 = () => (
    <p>
      2. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
      tempor incididunt ut labore et dolore magna aliqua.
    </p>
  );

  const Voting = withVoting(ElectionComponent, [BallotEntry1, BallotEntry2]); 

  return (
        <Voting
          electionProps={{
            appId: 1,
            row: 1,
            tableName: "example_election_table",
            name: "example",
            displayElection: true,
          }}
          ballotEntriesProps={[
            { tableName: "example_table", row: 42 },
            { tableName: "example_table", row: 23 },
          ]}
          direction="column-reverse"
          userId="userId"
        />
      )
}
```

### withCreateQuestion

Use this higher order component to create questions in your app.
Call the `withCreateQuestion` function and assign it to variable:

```tsx
const CreateQuestion = withCreateQuestion();
```

Required props for the `CreateQuestion` component:

```tsx
interface CreateQuestionProps {
  appId: number;
  userId: string;
  onSave: () => void;
  onCancel: () => void;
}
```

Example:

```tsx
import { withCreateQuestion } from "@eten-lab/voting";

function App() {
  const CreateQuestion = withCreateQuestion();

  return (
        <CreateQuestion
          appId={1} 
          userId="userId" 
          onSave={() => {}}
          onCancel={() => {}} 
        />
      )
}
```

### withSubmitQuestion

Use this higher order component to answer questions in your app.
Call the `withSubmitQuestion` function and assign it to variable:

```tsx
const SubmitQuestion = withSubmitQuestion();
```

Required props for the `SubmitQuestion` component:

```tsx
interface SubmitQuestionProps {
  questionId: number;
  userId: string;
  onSave: () => void;
  onCancel: () => void;
}
```

Example: 

```tsx
import { withSubmitQuestion } from "@eten-lab/voting";

function App() {
  const SubmitQuestion = withSubmitQuestion();

  return (
        <SubmitQuestion
          questionId={42}
          userId="userId"
          onSave={() => {}}
          onCancel={() => {}}
        />
      )
}
```



