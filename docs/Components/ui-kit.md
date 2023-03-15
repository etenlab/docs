# UI-Kit

## Description

This is our custom UI components library based on MUI.
You can see the components at the [Storybook server](https://storybook.dev.lab.eten.bible/)

## Installation

```sh
npm install --save @eten-lab/ui-kit
```

## Release

```sh
npm run release
```

This command automatically increate version of package and publish to `npmjs.org`.

## Usage

<CH.Code>

```tsx App.tsx
// focus
import { ThemeProvider } from '@eten-lab/ui-kit';
import { Home } from './Home';

export function App() {
  return (
    // focus
    <ThemeProvider>
      <Home />
      // focus
    </ThemeProvider>
  );
}
```

```tsx Home.tsx
import {
  MuiMaterial,
  Typography,
  colors,
  CiSearch,
  SearchInput,
  BiFile,
} from '@eten-lab/ui-kit';

// focus
// You can destructure every material components.
const { Stack, IconButton } = MuiMaterial;

export function Home() {
  return (
    <Stack>
      <Button>Sample Button</Button>
      <IconButton>Sample IconButton</IconButton>
    </Stack>
  );
}
```

</CH.Code>

Above example shows how to use ThemeProvider and components from @eten-lab/ui-kit.
You can find details for each component at the [Storybook server](https://storybook.dev.lab.eten.bible/).

### Palette

<CH.Code>

```typescript palette.ts
// This is definition file in ui-kit package.
export const colors = {
  'blue-primary': '#1F77DF',
  'light-blue': '#E3EAF3',
  disable: '#F3F6F9',
  dark: '#1B1B1B',
  gray: '#5C6673',
  'middle-gray': '#C2CBD7',
  white: '#FFFFFF',
  red: '#E04E4E',
  'light-red': '#FFE4E4',
  green: '#4ABE95',
  'light-green': '#DAF2EA',
  yellow: '#FCBB14',
  'middle-yellow': '#FFF1CE',
  'light-yellow': '#FFF9EA',
};
```

```tsx sample.tsx
import { MuiMaterial, Typography, colors, Button } from '@eten-lab/ui-kit';

export function Sample() {
  return <Button sx={{ color: colors['gray'] }}>Sample Button</Button>;
}
```

</CH.Code>

## How to Contribute

- Customize each component of MUI.

Example

```typescript packages/ui-kit/src/ThemeProvider/components.ts
import { ThemeOptions, PaletteColor } from '@mui/material';

export const components: ThemeOptions = {
  components: {
    MuiAutocomplete: {
      styleOverrides: {
        root: {
          '& .MuiInputBase-root': {
            paddingRight: '9px !important',
          },
          '& .MuiIconButton-sizeMedium': {
            padding: '3px !important',
          },
          '& label': {
            lineHeight: '1.1375em',
          },
          '& input': {
            paddingTop: '3.5px !important',
            paddingBottom: '3.5px !important',
          },
        },
      },
    },
  },
};
```

- Add Icons

Example

<CH.Code>

```tsx packages/ui-kit/src/icons/index.ts
export { CiDark, CiLight } from 'react-icons/ci';
```

```tsx stories/primary/icons/icons.stories.tsx
import React from 'react';
import { ComponentStory, ComponentMeta } from '@storybook/react';

// focus
import { CiDark, CiLight } from '../../../packages/ui-kit/src/icons';

export default {
  title: 'Primary/Icons',
  component: Div,
  decorators: [
    (Story) => (
      <div
        style={{
          margin: 'auto',
          background: '#eee',
          width: '500px',
        }}
      >
        <Story />
      </div>
    ),
  ],
} as ComponentMeta<typeof Div>;

const Template: ComponentStory<typeof Div> = (args) => {
  return (
    <Div>
      // focus
      <TitleWithIcon title="CiDark" icon={<CiDark />} />
      // focus
      <TitleWithIcon title="CiLight" icon={<CiLight />} />
    </Div>
  );
};

export const Primary = Template.bind({});
```

</CH.Code>

- Write Stories

Whenever you add new components to `ui-kit` package, you should write their stories in `stories` folder.

Example

```tsx Avatar.stories.tsx
import React, { useState, MouseEvent } from 'react';
import { ComponentStory, ComponentMeta } from '@storybook/react';

import { ThemeProvider } from '../../../../packages/ui-kit/src';
import { Avatar } from '../../../../packages/ui-kit/src/discussion-box';
import jsxToString from 'jsx-to-string';

export default {
  title: 'Partial/Discussion Box/Avatar',
  component: Avatar,
  decorators: [
    (Story) => (
      <div
        style={{
          margin: '3em',
          background: '#ccc',
          width: '500px',
          padding: '3em',
        }}
      >
        <ThemeProvider>
          <Story />
        </ThemeProvider>
      </div>
    ),
  ],
} as ComponentMeta<typeof Avatar>;

const Template: ComponentStory<typeof Avatar> = (args) => <Avatar {...args} />;

export const Text = Template.bind({});
Text.args = {
  username: 'Svetlana Podolianko',
};
Text.parameters = {
  docs: {
    source: {
      code: jsxToString(<Avatar username="Svetlana Podolianko" />),
      language: 'jsx',
      format: true,
      type: 'auto',
    },
  },
};

export const Picture = Template.bind({});
Picture.args = {
  username: 'Username',
  url: '/images.jpg',
};
Picture.parameters = {
  docs: {
    source: {
      code: jsxToString(<Avatar username="Username" url="/images.jpg" />),
      language: 'jsx',
      format: true,
      type: 'auto',
    },
  },
};

export const Mini = Template.bind({});
Mini.args = {
  username: 'Username',
  url: '/images.jpg',
  mini: true,
};
Mini.parameters = {
  docs: {
    source: {
      code: jsxToString(<Avatar username="Username" url="/images.jpg" mini />),
      language: 'jsx',
      format: true,
      type: 'auto',
    },
  },
};
```

## Source

- [Github Repository](https://github.com/etenlab/ui-kit)
- [Storybook server](https://storybook.dev.lab.eten.bible/)
