# Problem with Vite and CommonJS package in monorepo

In this repo we have basic monorepo with 3 packages

- Vite app (`/packages/app`)
- CommonJS library (`/packages/lib-cjs`)
- ESM Library (`/packages/lib-esm`)

Vite app depends on both libraries and also a copy of CommonJS library is installed from github (https://github.com/doomsower/vite-cjs-test-lib)

Vite app builds and launches successfully if I use ESM library or CommonJS library from github.  
Hovewer, with local cjs library ([import statement](https://github.com/doomsower/vite-mono-test/blob/master/packages/app/src/App.tsx#L4)) `vite build` fails with error:

`'test' is not exported by ../lib-cjs/dist/index.js, imported by src/App.tsx`

I tried various modification to `vite.config.ts` but none seem to work

<details>
  <summary>Full error log</summary>
  
  ```
  pnpm build                                                                          20:56:16 
  
  > @vite-mono/app@0.0.0 build /Volumes/Projects/_temp/vite-mono/packages/app
  > tsc && vite build
  
  vite v2.6.14 building for production...
  transforming (30) ../../node_modules/.pnpm/scheduler@0.20.2/node_modules/scheduler/cjs/scheduler.production.min.jsError when using sourcemap for reporting an error:   Can't resolve original location of error.
  ✓ 34 modules transformed.
  'test' is not exported by ../lib-cjs/dist/index.js, imported by src/App.tsx
  file: /Volumes/Projects/_temp/vite-mono/packages/app/src/App.tsx:4:9
  2: import logo from "./logo.svg";
  3: import "./App.css";
  4: import { test } from "@vite-mono/lib-cjs";
              ^
  5: import { jsx as _jsx } from "react/jsx-runtime";
  6: import { jsxs as _jsxs } from "react/jsx-runtime";
  error during build:
  Error: 'test' is not exported by ../lib-cjs/dist/index.js, imported by src/App.tsx
      at error (/Volumes/Projects/_temp/vite-mono/node_modules/.pnpm/rollup@2.60.0/node_modules/rollup/dist/shared/rollup.js:158:30)
      at Module.error (/Volumes/Projects/_temp/vite-mono/node_modules/.pnpm/rollup@2.60.0/node_modules/rollup/dist/shared/rollup.js:12382:16)
      at Module.traceVariable (/Volumes/Projects/_temp/vite-mono/node_modules/.pnpm/rollup@2.60.0/node_modules/rollup/dist/shared/rollup.js:12767:29)
      at ModuleScope.findVariable (/Volumes/Projects/_temp/vite-mono/node_modules/.pnpm/rollup@2.60.0/node_modules/rollup/dist/shared/rollup.js:11559:39)
      at FunctionScope.findVariable (/Volumes/Projects/_temp/vite-mono/node_modules/.pnpm/rollup@2.60.0/node_modules/rollup/dist/shared/rollup.js:6930:38)
      at ChildScope.findVariable (/Volumes/Projects/_temp/vite-mono/node_modules/.pnpm/rollup@2.60.0/node_modules/rollup/dist/shared/rollup.js:6930:38)
      at Identifier.bind (/Volumes/Projects/_temp/vite-mono/node_modules/.pnpm/rollup@2.60.0/node_modules/rollup/dist/shared/rollup.js:6419:40)
      at CallExpression.bind (/Volumes/Projects/_temp/vite-mono/node_modules/.pnpm/rollup@2.60.0/node_modules/rollup/dist/shared/rollup.js:5025:23)
      at CallExpression.bind (/Volumes/Projects/_temp/vite-mono/node_modules/.pnpm/rollup@2.60.0/node_modules/rollup/dist/shared/rollup.js:9396:15)
      at TemplateLiteral.bind (/Volumes/Projects/_temp/vite-mono/node_modules/.pnpm/rollup@2.60.0/node_modules/rollup/dist/shared/rollup.js:5021:31)
   ELIFECYCLE  Command failed with exit code 1.
  ```
</details>

<details>
  <summary>Pnpm links</summary>

```
ls -la node_modules/@vite-mono                                                        20:49:33 
total 24
drwxr-xr-x   5 doomsower  admin  170 Nov 13 20:47 .
drwxr-xr-x  11 doomsower  admin  374 Nov 13 20:47 ..
lrwxr-xr-x   1 doomsower  admin   16 Nov 13 19:37 lib-cjs -> ../../../lib-cjs
lrwxr-xr-x   1 doomsower  admin  149 Nov 13 20:47 lib-cjs-remote -> ../../../../node_modules/.pnpm/github.com+doomsower+vite-cjs-test-lib@8c6e40dc2c8098d23a84f498dd1022d30f03ef3c/node_modules/@vite-mono/lib-cjs-remote
lrwxr-xr-x   1 doomsower  admin   16 Nov 13 19:37 lib-esm -> ../../../lib-esm
```

</details>
