## Integration crowd.Bible to platform.Bible as extension
- this concept is under development / reserarch yet.
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

In common
- removed html web view file (lib/extension-template-html.web-view.ejs) and cleaned up from code - we don't need it.
- renamed paranext-extension-template to crowd-bible-extension everywhere
- simplified/removed work with DataProviderEngine at main.ts - temporary, we don't need it yet, until we fugure out how to deal with data interchange.
- copied all code of crowd.Bible to /src
- moved all context providers from index.ts to App.tsx to make App.tsx as entry point for extension. index.ts is not needed anymore, but left as example.
- temporary removed all context and all logic, left only basic button imported from ui-kit  and text field to prove the posibility of using ui-kit components.
- further work is to return one-by-one context providers and components.


See repo's [https://github.com/etenlab/extension-paranext] commit history for more details.

### Known problems and ways to tackle them

1. Error on build like
`[ImportManager] Cannot read properties of undefined (reading 'at')`
 This issue is related to building into multiple files. Need to disable code splitting into chunks.
(look at [https://github.com/etenlab/extension-paranext/pull/1/files#diff-b85227f55b07b40c5cbd7482461215e25c6a23d72b0abcbf1a4061ff26b6bffc] )

2. Despite module supports automatic rebuilding on the fly when changed, it is recommend to stop the whole paranext-core and rebuild using `npm run start`.

3. If you get app running but with empty extension's window, you most likely have errors on your extension's code. Probably, they're realted to absence of some `env` variables. Include them into `vite-web-view.config.ts`