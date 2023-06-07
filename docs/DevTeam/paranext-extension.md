# Paranext extension
## Integration crowd.Bible to platform.Bible as extension
- this concept is under development / research yet.
- here we note gathered experience of existing problems and known solutions.

### Links 

Paranext core [https://github.com/paranext/paranext-core]. Look here for details about Paranext core.

Paranext extension template  [https://github.com/paranext/paranext-extension-template]. Look here for details about Paranext extension template.

Our experimental extension [https://github.com/etenlab/extension-paranext]

Notes: 
- Our experimental extension is made from Paranext extension template by cleaning up redundant code and adding some configuration.
- Also some stable state of the crowd.Bible app was copied to /src folder of Our experimental extension. Our goal is to make this code working. But no luck with starting it "as is", so we cleaned up all logic and components and try to add them step-by-step.

### How to make it running

1. Make some folder to work (e.g. /paranext)

2. Clone Paranext core to folder `/paranext/paranext-core` and install necessary tools using instructions https://github.com/paranext/paranext-core

3. Clone our experimental extension to folder `/paranext/crowd-bible-extension`

4. cd to `/paranext/crowd-bible-extension` and run `npm install` and then `npm run start` to start paranext core with this extension. Paranext core starts atumatically despite you make `npm run start` startting from extension's folder.

Note: you may run `npm run build:vite` just to build extension without starting paranext core.

### What we already did 
We forked Paranext extension template  form [https://github.com/paranext/paranext-extension-template] and changed it to make Our experimental extension. 

There we:
- removed html web view file (lib/extension-template-html.web-view.ejs) and cleaned up from code - we don't need it.
- renamed paranext-extension-template to crowd-bible-extension everywhere
- simplified/removed work with DataProviderEngine at main.ts - temporary, we don't need it yet, until we fugure out how to deal with data interchange.
- copied all code of crowd.Bible to /src
- moved all context providers from index.ts to App.tsx to make App.tsx as entry point for extension. index.ts is not needed anymore, but left as example.
- temporary removed all context and all logic, left only basic button imported from ui-kit  and text field to prove the posibility of using ui-kit components.
- we use absolute path alias in crowd.Bible, so we tuned up paths resolution `vite-web-view.config.ts` by adding  alias:
```
    ...
    resolve: {
      alias: {
        '@': path.resolve(__dirname, '../lib/src'),
      },
    }, 
    ...
```
- to use .env variables, the next tuning of `vite-web-view.config.ts` should be applied:
```
    ...
    define: { 
      'process.env.NODE_ENV': JSON.stringify(mode),
      // There were a lot of process variables that we must bake in. Set them to whatever is
      // correct for your build process
      'process.env.REACT_APP_CPG_SERVER_URL': '"FAKE_URL"',
      'process.env.REACT_APP_KEYCLOAK_URL': '"FAKE_URL"',
      'process.env.REACT_APP_KEYCLOAK_REALM': '"FAKE_REALM"',
      'process.env.REACT_APP_KEYCLOAK_CLIENT_ID': '"FAKE_CID"',
      'process.env.REACT_APP_KEYCLOAK_CLIENT_SECRET': '"FAKE_SECRET"',
      'process.env.PUBLIC_URL': '"FAKE_URL"',
      'process.env.REACT_APP_LOGLEVEL': '0',
      'process.env.REACT_APP_LOGTRACEALL': 'true',
      'process.env.STORYBOOK_LOGLEVEL': '0',
      'process.env.STORYBOOK_LOGTRACEALL': 'true',
    },
    ...
```
- now we can bring back ThemeProvider, IonReactRouter and PageLayout. It can be built and it renders, but still thows errors wher try to navigate.
- further work is to return one-by-one context providers like KeyCloak, state and state management, other components.


See repo's [https://github.com/etenlab/extension-paranext] commit history for more details.

### Known problems and ways to tackle them

1. Error on build like
`[ImportManager] Cannot read properties of undefined (reading 'at')`
 This issue is related to building into multiple files. Need to disable code splitting into chunks.
(look at [https://github.com/etenlab/extension-paranext/pull/1/files#diff-b85227f55b07b40c5cbd7482461215e25c6a23d72b0abcbf1a4061ff26b6bffc] )

2. Despite module supports automatic rebuilding on the fly when changed, it is recommend to stop the whole paranext-core and rebuild using `npm run start`.

3. If you get app running but with empty extension's window, you most likely have errors on your extension's code. Probably, they're realted to absence of some `env` variables. Include them into `vite-web-view.config.ts`

4. Error on build like
`[ImportManager] Cannot read properties of undefined (reading 'at')` when atempt to nring back singletons most likely caused by initializing sql.js at `data-source.ts` because of downloading and running wasm file.
possible (not checked yet) thoughts:
```
 https://sql.js.org/#/
By default, sql.js uses wasm, and thus needs to load a .wasm file in addition to the javascript library. You can find this file in ./node_modules/sql.js/dist/sql-wasm.wasm after installing sql.js from npm, and instruct your bundler to add it to your static assets or load it from a CDN. Then use the locateFile property of the configuration object passed to initSqlJs to indicate where the file is. If you use an asset builder such as webpack, you can automate this.
```

or  maybe folloow TJ Couch's advice:
```
Looks like you may need to allow access to https: for connect-src and wasm-unsafe-eval on script-src in the content security policies in index.ejs and web-view.service.ts. Maybe try that and see what happens?

I don't know if we will be comfortable enabling any unsafe-eval-like content security policies or not. We will have to discuss this.
```

Note: all errors on build stage are represented as `[ImportManager] Cannot read properties of undefined (reading 'at')` - it is significantly slows debugging process, making barely possible to understand what's gone wrong.

Note2: After changing something and restarting the app, it starts electron app and build process concurrently, so appearing changes are syncronous, so you have to wait some time and don't make conclusions too early.