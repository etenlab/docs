# crowd.bible

## Description

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

function Sample({ userId }: { userId: number; }) {
  const { actions: { alertFeedback } } = useAppContext();

  if (userId === null) {
    alertFeedback("warning", "Please login!");
    return null;
  }

  return <div>{userId}</div>
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

## Source

- [Github Repository](https://github.com/etenlab/crowd.Bible)
- [Crowd Bibile App](https://crowdbible.dev.lab.eten.bible/welcome)
