# Test case for storybook "macro" issue

The issue is that you cannot use a TypeScript module named "macro" when running storybook.  It produces a `Cannot find module` error.

## Setup

1. `npx create-react-app storybook-macro-issue --template typescript`
2. `cd storybook-macro-issue && npx sb init`
3. Add a `macro.ts` file and import it in a file used by a story.
4. `npm run storybook`

Storybook will run, but any stories that use `macro.ts` will be absent.

## Full error

```
WARNING in ./src/stories/Button.stories.tsx
Module build failed (from ./node_modules/babel-loader/lib/index.js):
Error: <storybook-macro-issue>\src\stories\Button.stories.tsx: Cannot find module '../macro' from '<storybook-macro-issue>\src\stories'
    at Function.resolveSync [as sync] (<storybook-macro-issue>\node_modules\resolve\lib\sync.js:89:15)
    at nodeResolvePath (<storybook-macro-issue>\node_modules\babel-plugin-macros\dist\index.js:68:18)
    at applyMacros (<storybook-macro-issue>\node_modules\babel-plugin-macros\dist\index.js:207:23)
    at ImportDeclaration (<storybook-macro-issue>\node_modules\babel-plugin-macros\dist\index.js:114:28)
    at NodePath._call (<storybook-macro-issue>\node_modules\@babel\traverse\lib\path\context.js:55:20)
    at NodePath.call (<storybook-macro-issue>\node_modules\@babel\traverse\lib\path\context.js:42:17)
    at NodePath.visit (<storybook-macro-issue>\node_modules\@babel\traverse\lib\path\context.js:92:31)
    at TraversalContext.visitQueue (<storybook-macro-issue>\node_modules\@babel\traverse\lib\context.js:115:16)
    at TraversalContext.visitMultiple (<storybook-macro-issue>\node_modules\@babel\traverse\lib\context.js:79:17)
    at TraversalContext.visit (<storybook-macro-issue>\node_modules\@babel\traverse\lib\context.js:141:19)
    at Function.traverse.node (<storybook-macro-issue>\node_modules\@babel\traverse\lib\index.js:82:17)
    at traverse (<storybook-macro-issue>\node_modules\@babel\traverse\lib\index.js:64:12)
    at NodePath.traverse (<storybook-macro-issue>\node_modules\@babel\traverse\lib\path\index.js:142:24)
    at PluginPass.Program (<storybook-macro-issue>\node_modules\babel-plugin-macros\dist\index.js:95:18)
    at newFn (<storybook-macro-issue>\node_modules\@babel\traverse\lib\visitors.js:175:21)
    at NodePath._call (<storybook-macro-issue>\node_modules\@babel\traverse\lib\path\context.js:55:20)
 @ \.)(?=.)[^\\/]*?\.stories\.(js|jsx|ts|tsx))$ (./src sync ^\.(?:(?:^|[\\/]|(?:(?:(?!(?:^|[\\/])\.).)*?)[\\/])(?!\.)(?=.)[^\\/]*?\.stories\.(js|jsx|ts|tsx))$) ./stories/Button.stories.tsx
 @ ./.storybook/generated-stories-entry.js
 @ multi ./node_modules/@pmmmwh/react-refresh-webpack-plugin/client/ReactRefreshEntry.js ./node_modules/react-dev-utils/webpackHotDevClient.js ./node_modules/@storybook/core/dist/server/common/polyfills.js ./node_modules/@storybook/core/dist/server/preview/globals.js ./.storybook/storybook-init-framework-entry.js ./node_modules/@storybook/addon-docs/dist/frameworks/common/config.js-generated-other-entry.js ./node_modules/@storybook/addon-docs/dist/frameworks/react/config.js-generated-other-entry.js ./node_modules/@storybook/addon-links/dist/preset/addDecorator.js-generated-other-entry.js ./node_modules/@storybook/addon-actions/dist/preset/addDecorator.js-generated-other-entry.js ./node_modules/@storybook/addon-actions/dist/preset/addArgs.js-generated-other-entry.js ./node_modules/@storybook/addon-backgrounds/dist/preset/addDecorator.js-generated-other-entry.js ./node_modules/@storybook/addon-backgrounds/dist/preset/addParameter.js-generated-other-entry.js ./.storybook/preview.js-generated-config-entry.js ./.storybook/generated-stories-entry.js (webpack)-hot-middleware/client.js?reload=true&quiet=false&noInfo=undefined
```

## Possible cause

It seems likely that this `babel-plugin-macros` is involved.  It uses a regex to detect some kind of special imports, and that regex would catch any path ending in `/macro`.