# crowd.bible

## Description

## How to Run
### Cloning the App

```bash
$ git clone https://github.com/etenlab/crowd.Bible.git
```

### Install dependencies

```bash
$ npm i
```

### Running the App

```bash
$ ionic serve
```


## How to Contribute

We are using custom react component library built with MUI material v5, React-Quill, React-icons.

### Convention

- Utilize absolute paths

<CH.Code>

```json tsconfig.paths.json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/src/*": ["src/*"],
      "@/components/*": ["src/components/*"],
      "@/hooks/*": ["src/hooks/*"],
      "@/models/*": ["src/models/*"],
      "@/pages/*": ["src/pages/*"],
      "@/reducers/*": ["src/reducers/*"],
      "@/repositories/*": ["src/repositories/*"],
      "@/router/*": ["src/router/*"],
      "@/services/*": ["src/services/*"],
      "@/utils/*": ["src/utils/*"]
    }
  }
}
```

```tsx App.tsx
import { WelcomePage } from '@/pages/WelcomePage';
import { AppContextProvider } from '@/src/AppContext';
import { PageLayout } from '@/components/PageLayout';
import { ProtectedRoutes } from '@/router/ProtectedRoutes';
```

</CH.Code>

### Components & Functions

- alertFeedback(feedbackType: 'success' | 'error' | 'info' | 'warning', message: string )

This is a alert function.

```tsx sample.tsx
import { useAppContext } from '@/hooks/useAppContext';

function Sample({ userId }: { userId: number }) {
  const {
    actions: { alertFeedback },
  } = useAppContext();

  if (userId === null) {
    alertFeedback('warning', 'Please login!');
    return null;
  }

  return <div>{userId}</div>;
}
```

### Do not & Do

- Don't install directly Mui package.

Do not

```tsx
import { Button } from '@mui/material';

function App() {
  return <Button>Sample Button</Button>;
}
```

Do

```tsx
import { MuiMaterial } from '@eten-lab/ui-kit';

const { Button } = MuiMaterial;

function App() {
  return <Button>Sample Button</Button>;
}
```

- Dont't use literal color in style props or sx.

Do not

```tsx
function App() {
  return <Button sx={{ color: '#fff' }}>Sample Button</Button>;
}
```

Do

```tsx sample.tsx
import { useColorModeContext } from '@eten-lab/ui-kit';

function App() {
  const { getColor } = useColorModeContext();

  return <Button sx={{ color: getColor('white') }}>Sample Button</Button>;
}
```

- Dont't use literal color in styled.ts.

Do not

```tsx
import { MuiMaterial } from '@eten-lab/ui-kit';

const { styled } = MuiMaterial;

export const CustomButton = styled('div')({
  color: '#fff',
  background: '#000',
  ...
})
```

Do

```tsx
import { MuiMaterial } from '@eten-lab/ui-kit';

const { styled } = MuiMaterial;

export const CustomButton = styled('div')(({ theme }) => ({
  color: theme.palette.text.white,
  background: theme.palette.text.dark,
  ...
}))
```

- Dont't set color for Typography component with sx or style.

Do not

```tsx
import { MuiMaterial, useColorModeContext } from '@eten-lab/ui-kit';

const { Typography } = MuiMaterial;

function App() {
  const { getColor } = useColorModeContext();

  return (
    <Typography variant="h1" sx={{ color: getColor('white') }}>
      Heading 1
    </Typography>
  );
}
```

Do

```tsx
import { MuiMaterial } from '@eten-lab/ui-kit';

const { Typography } = MuiMaterial;

function App() {
  return (
    <Typography variant="h1" color="text.white">
      Heading 1
    </Typography>
  );
}
```

## About Voting Service

### @/hooss/useVote.ts

```typescript
/**
 * This hooks provides every methods for voting system.
 */
export function useVote() {
  /**
   * This function get Election array by tableName and rowId
   */
  const listElections = async (tableName: TablesName, rowId: Nanoid) => {};

  /**
   * This function get all ballotEntries with voting status
   */
  const getElectionFull = async (electionId: Nanoid) => {};

  /**
   * This function create an Election and return uuid.
   */
  const createElection = async (tableName: TablesName, rowId: Nanoid) => {};

  /**
   * This function add BallotEntry to an election specified by param.
   */
  const addBallotEntry = async (
    electionId: Nanoid,
    ballotEntryTarget: BallotEntryTarget,
  ) => {};

  /**
   * A user can change their vote state via this function.
   */
  const addVote = async (ballotEntryId: Nanoid, vote: boolean | null) => {};

  /**
   * This function will toggle vote state with given vote.
   */
  const toggleVote = async (ballotEntryId: Nanoid, vote: boolean | null) => {};

  /**
   * This function get Ballot Stats by given id.
   */
  const getVotesStats = async (ballotEntryId: Nanoid) => {};

  /**
   *
   */
  const getBallotEntryId = async (
    electionId: Nanoid,
    ballotEntryTarget: BallotEntryTarget,
  ) => {};

  return {
    createElection,
    listElections,
    getElectionFull,
    getVotesStats,
    addBallotEntry,
    getBallotEntryId,
    toggleVote,
    addVote,
  };
}
```

### Demo

Here is an example of how to use the `useVote` hook to implement a voting system.

At [/translation](https://www.figma.com/file/md8nAIVfMUNxGhkofGhP6c/Wireframes?node-id=1643-9140&t=pzy1zHh2c3rZVV2w-0) page, translators select `word-sequence` which want to translate. And save translations.

A translator can make multiple translations for a sequence of words, and other translators can do the same.

Using a voting system, translators decide what is the best translation in terms of word sequences.

Elections were held by choosing a word sequence.
Election nodes can be created via the `createElection` function, which requires the parameter `nodes` and the `uuid` of the word sequence.

And the various translations of the original word sequence are `ballot entry` in the election

Whenever translators write a new translation, a new ballot entities will be created via `addBallotEntry` function.

[upvote or downvote](https://www.figma.com/file/md8nAIVfMUNxGhkofGhP6c/Wireframes?node-id=1643-9217&t=pzy1zHh2c3rZVV2w-0)
There are `upvote` and `downvote` buttons that allow users to vote by clicking on these buttons.

The upvoting button can be done through the `toggleVote` function.

If the vote is successful, the voting status should be updated with the updated vote. you can do this via `getVotesStats` function.

<CH.Spotlight>

<CH.Code>

```typescript @/components/TranslationEditor/TranslationEditor.tsx
const handleSaveTranslation = async () => {
  let subWordSequenceId: Nanoid;
  let electionId: Nanoid;

  if (typeof subWordSequence !== 'string') {
    const wordSequenceId = await createSubWordSequence(
      subWordSequence.origin,
      subWordSequence.range,
    );

    if (!wordSequenceId) {
      return;
    }

    const tmpElectionId = await createElection('nodes', wordSequenceId);

    if (!tmpElectionId) {
      return;
    }

    electionId = tmpElectionId;

    subWordSequenceId = wordSequenceId;
  } else {
    subWordSequenceId = subWordSequence;

    electionId = (await listElections('nodes', subWordSequenceId))[0];
  }

  const translationId = await createTranslation(subWordSequenceId, text);

  if (!translationId) {
    return;
  }

  await addBallotEntry(electionId, {
    tableName: 'nodes',
    rowId: translationId,
  });

  history.push(`/translation/${documentId}`);
};
```

```typescript @/hooks/useWordSequence.ts
interface IndexObject {
  [key: string]: {
    electionId: string;
    ballotEntryIds: {
      [key: string]: string;
    };
  };
}

// This variable caching every electionIds and ballotEntryIds
const electionIds: IndexObject = {};

const appendVoteInfoToTranslation = useCallback(
  async (translationDtos: WordSequenceDto[]) => {
    const translationWithVoteDtos: WordSequenceWithVote[] = [];

    for (const translation of translationDtos) {
      const originId = translation.originalWordSequenceId!;

      if (electionIds[originId] === undefined) {
        const tmpElectionIds = await listElections('nodes', originId);

        if (tmpElectionIds.length === 0) {
          continue;
        }

        electionIds[originId] = {
          electionId: tmpElectionIds[0],
          ballotEntryIds: {},
        };
      }

      if (electionIds[originId].ballotEntryIds[translation.id] === undefined) {
        const ballotEntryId = await getBallotEntryId(
          electionIds[originId].electionId,
          {
            tableName: 'nodes',
            rowId: translation.id,
          },
        );

        if (!ballotEntryId) {
          continue;
        }

        electionIds[originId].ballotEntryIds[translation.id] = ballotEntryId;
      }

      let vote = await getVotesStats(
        electionIds[originId].ballotEntryIds[translation.id],
      );

      if (!vote) {
        vote = {
          ballot_entry_id: electionIds[originId].ballotEntryIds[translation.id],
          up: 0,
          down: 0,
        };
      }
      translationWithVoteDtos.push({
        ...translation,
        vote,
      });
    }

    return translationWithVoteDtos;
  },
  [getVotesStats, getBallotEntryId, listElections],
);
```

```typescript @/utils/types.d.ts
type VotesStatsRow = {
  ballot_entry_id: Nanoid;
  up: number;
  down: number;
};
```

---

```tsx @/components/TranslationList/TranslationList.tsx
function Voting({
  vote,
  onChangeVote,
}: {
  vote: VotesStatsRow;
  onChangeVote(): void;
}) {
  const { toggleVote } = useVote();

  const handleToggleVote = async (voteValue: boolean) => {
    await toggleVote(vote.ballot_entry_id, voteValue);
    onChangeVote();
  };

  return (
    <Stack direction="row" gap="20px">
      <VoteButton count={vote.up} onClick={() => handleToggleVote(true)} />
      <VoteButton
        isLike={false}
        count={vote.down}
        onClick={() => handleToggleVote(false)}
      />
    </Stack>
  );
}

function Translation({
  translation,
  isCheckbox,
  onChangeVote,
}: {
  translation: WordSequenceWithVote;
  isCheckbox: boolean;
  onChangeVote: (translationId: Nanoid, ballotEntryId: Nanoid) => void;
}) {
  const { id, wordSequence, vote } = translation;

  const { getColor } = useColorModeContext();

  const handleClickDiscussionButton = () => {
    // history.push(`/discussion/table-name/${text}/row/${id}`);
  };

  const checkbox = isCheckbox ? <Checkbox sx={{ marginLeft: '-9px' }} /> : null;

  return (
    <>
      <Stack
        direction="row"
        alignItems="flex-start"
        sx={{ marginBottom: '12px', width: '100%' }}
      >
        {checkbox}
        <Stack gap="3px" sx={{ width: '100%' }}>
          <Typography
            variant="body3"
            sx={{ padding: '9px 0', color: getColor('dark') }}
          >
            {wordSequence}
          </Typography>
          <Stack
            direction="row"
            justifyContent="space-between"
            alignItems="center"
          >
            <Voting
              vote={vote}
              onChangeVote={() => onChangeVote(id, vote.ballot_entry_id)}
            />

            <IconButton onClick={handleClickDiscussionButton}>
              <BiMessageRounded
                style={{
                  padding: '5px',
                  borderRadius: '4px',
                  background: getColor('light-blue'),
                  color: getColor('gray'),
                  fontSize: '26px',
                }}
              />
            </IconButton>
          </Stack>
        </Stack>
      </Stack>
      <Divider />
    </>
  );
}

interface TranslationListProps {
  documentId: Nanoid | null;
  wordSequenceId: Nanoid | null;
  isCheckbox?: boolean;
}

export function TranslationList({
  documentId,
  wordSequenceId,
  isCheckbox = true,
}: TranslationListProps) {
  const {
    listTranslationsByDocumentId,
    listTranslationsByWordSequenceId,
    listMyTranslationsByDocumentId,
    listMyTranslationsByWordSequenceId,
  } = useWordSequence();
  const { getVotesStats } = useVote();
  const {
    states: {
      global: { singletons },
    },
  } = useAppContext();
  const [currentTab, setCurrentTab] = useState<'all' | 'mine'>('all');
  const [translations, setTranslations] = useState<WordSequenceWithVote[]>([]);

  useEffect(() => {
    if (!documentId || !singletons) {
      return;
    }

    if (!wordSequenceId) {
      if (currentTab === 'all') {
        listTranslationsByDocumentId(documentId).then(setTranslations);
      } else if (currentTab === 'mine') {
        listMyTranslationsByDocumentId(documentId).then(setTranslations);
      }
    } else {
      if (currentTab === 'all') {
        listTranslationsByWordSequenceId(wordSequenceId).then(setTranslations);
      } else if (currentTab === 'mine') {
        listMyTranslationsByWordSequenceId(wordSequenceId).then(
          setTranslations,
        );
      }
    }
  }, [
    singletons,
    documentId,
    wordSequenceId,
    currentTab,
    listTranslationsByDocumentId,
    listTranslationsByWordSequenceId,
    listMyTranslationsByWordSequenceId,
    listMyTranslationsByDocumentId,
  ]);

  const handleTabChange = (
    _event: React.SyntheticEvent,
    newValue: 'all' | 'mine',
  ) => {
    setCurrentTab(newValue);
  };

  const addMyTranslationComponent =
    currentTab === 'mine' ? (
      <Link to={`/translation-edit/${documentId}/${wordSequenceId}`}>
        <Button
          variant="contained"
          startIcon={<FiPlus />}
          fullWidth
          sx={{ margin: '10px 0' }}
        >
          Add My Translation
        </Button>
      </Link>
    ) : null;

  const handleChangeVote = async (
    translationId: Nanoid,
    ballotEntryId: Nanoid,
  ) => {
    const vote = await getVotesStats(ballotEntryId);

    if (!vote) {
      return;
    }

    setTranslations((translations) =>
      translations.map((translation) => {
        if (translation.id === translationId) {
          return {
            ...translation,
            vote,
          };
        }
        return translation;
      }),
    );
  };

  return (
    <>
      <Tabs
        tabs={[
          { value: 'all', label: 'All Translations' },
          { value: 'mine', label: 'My Translations' },
        ]}
        value={currentTab}
        onChange={handleTabChange}
      />
      {addMyTranslationComponent}

      <Stack sx={{ flexGrow: 1, overflowY: 'auto' }}>
        {translations.map((item) => (
          <Translation
            key={item.id}
            translation={item}
            isCheckbox={isCheckbox}
            onChangeVote={handleChangeVote}
          />
        ))}
      </Stack>
    </>
  );
}
```

</CH.Code>

---

#### If there isn't WordSequence

---

- Create WordSequence

```typescript @/components/TranslationEditor/TranslationEditor.tsx focus=6:14

```

---

- Create Election

```typescript @/components/TranslationEditor/TranslationEditor.tsx focus=14:20

```

---

#### If there is WordSequence

---

- Get Election Id by Given WordSequence Id

```typescript @/components/TranslationEditor/TranslationEditor.tsx focus=26:27

```

---

#### Add Ballot Entry

---

- Create Translation WordSequence

```typescript @/components/TranslationEditor/TranslationEditor.tsx focus=30:35

```

---

- Attach a new Ballot Entry into the election

```typescript @/components/TranslationEditor/TranslationEditor.tsx focus=35:40

```

---

#### Get List of Translations with Voting Status

---

- Declare a variable for caching Election and Ballot Entry Ids

```typescript @/hooks/useWordSequence.ts focus=1:12

```

---

- Find Election Id by Original WordSequenceId

```typescript @/hooks/useWordSequence.ts focus=18:31

```

---

- Find BallotEntry Id by ElectionId and Translation WordSequence Id

```typescript @/hooks/useWordSequence.ts focus=33:47

```

---

- Get Vote Status By Ballot Entry Id

```typescript @/hooks/useWordSequence.ts focus=49:51

```

---

- Attach voting status to translation obj

```typescript @/hooks/useWordSequence.ts focus=53:63

```

---

#### Render Vote Buttons

---

- Define VoteStatsRow type

```typescript @/utils/types.d.ts focus=1:5

```

---

- Voting Component Params

```tsx @/components/TranslationList/TranslationList.tsx focus=1:7

```

---

- Define toggle vote handler

```tsx @/components/TranslationList/TranslationList.tsx focus=8:13

```

---

- Upvote button

```tsx @/components/TranslationList/TranslationList.tsx focus=17:18

```

---

- Downvote button

```tsx @/components/TranslationList/TranslationList.tsx focus=18:22

```

---

- Use Voting component at Translation Component

```tsx @/components/TranslationList/TranslationList.tsx focus=66:69

```

---

- Update Translation vote status with updated one

```tsx @/components/TranslationList/TranslationList.tsx focus=168:188

```

---

- Use Translation Component at TranslationList Component

```tsx @/components/TranslationList/TranslationList.tsx focus=205:210

```

</CH.Spotlight>

## Source

- [Github Repository](https://github.com/etenlab/crowd.Bible)
- [Crowd Bibile App](https://crowdbible.dev.lab.eten.bible/welcome)
