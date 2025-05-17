# Bug #1 repro instructions

1. Clone this repo, and checkout the `main` branch.
2. Within `project1/`, run `yarn install`.
3. Within `project2/`, run `yarn install`.
4. Within `project2/`, run `yarn tsc --build --watch`. There should be no compile errors.
5. While the `--watch` command is running, in another terminal window run `git checkout multiply-function`.
    - the `multiply-function` branch adds a commit that:
        - adds `project1/src/multiply.ts` which exports a `multiply` function
        - and edits `project2/src/main.ts` to import the `multiply` function and use it

Now, the `--watch` process will show the error:
```
src/main.ts:3:24 - error TS6059: File '/Users/ericchen/Anrok/codez/ts-watch-bug-repro/project1/src/multiply.ts' is not under 'rootDir' '/Users/ericchen/Anrok/codez/ts-watch-bug-repro/project2/src'. 'rootDir' is expected to contain all source files.

3 import {multiply} from 'project1/multiply';
                         ~~~~~~~~~~~~~~~~~~~

[6:16:54 PM] Found 1 error. Watching for file changes.
```

In addition, 2 stray `.js`/`.js.map` files are created:
```
project1/src/multiply.js
project1/src/multiply.js.map
```

# Bug #2 repro instructions

1. Clone this repo, and checkout the `multiply-function` branch.
2. Within `project1/`, run `yarn install`.
3. Within `project2/`, run `yarn install`.
4. Within `project2/`, run `yarn tsc --build --watch`. There should be no compile errors.
5. While the `--watch` command is running, in another terminal window run `git checkout main`.
    - this will:
        - delete the file `project1/src/multiply.ts`, which exported a `multiply` function
        - and edit `project2/src/main.ts` to remove the import and usage of `multiply`

Now, the `--watch` process will show the error:
```
[10:54:43 PM] File change detected. Starting incremental compilation...

error TS6053: File '/Users/ericchen/Anrok/codez/ts-watch-bug-repro/project1/src/multiply.ts' not found.
  The file is in the program because:
    Matched by include pattern 'src/**/*.ts' in '/Users/ericchen/Anrok/codez/ts-watch-bug-repro/project1/tsconfig.json'

  ../project1/tsconfig.json:20:17
    20     "include": ["src/**/*.ts"],
                       ~~~~~~~~~~~~~
    File is matched by include pattern specified here.

[10:54:43 PM] Found 1 error. Watching for file changes.
```
