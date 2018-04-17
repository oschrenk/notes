# Typescript

Install `npm install -g typescript`

TypeScript comes with two binaries: `tsc`, the TypeScript compiler and `tsserver`.

To *initialize a TypeScript project*, simply use

```
$ tsc --init
message TS6071: Successfully created a tsconfig.json file.
```

It will create a default `tsconfig.json` file

```
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true
  }
}
```

Let’s add some more options to that base configuration:
```
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "outDir": "dist",
    "rootDir": "src/main/typescript",
    "sourceMap": true
  }
}
```

* `outDir` instructs `tsc` to generate JavaScript files in that directory (relative to `tsconfig.json`)
* `rootDir` instructs `tsc` to look for source files in that directory (relative to `tsconfig.json`. I choose `src/main/typescript` because it is a shared Scala and Typescript project and I like the Maven way of ordering things.
* `sourceMap` instructs  `tsc` to create source maps

So let's create `src/main/typescript/index.ts` with
```
const world = '🗺️';

export function hello(word: string = world): string {
  return `Hello ${world}! `;
}
```

and then run the compiler in root with `tsc` to generate source files and source maps in `dist`

Instead of re-running `tsc` you can start *watch mode*, automatically re-running `tsc` whenever a file has changed. Do that with `tsc -w`
