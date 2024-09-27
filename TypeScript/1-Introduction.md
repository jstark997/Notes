# TypeScript - Introduction

TypeScript is a typed superset of JavaScript that compiles to plain JavaScript. It adds static types, interfaces, and other features to JavaScript.

- File extension: `.ts`
- Requires Node.js to be installed in order to compile.
- To install (once Node.js has also been installed):

```sh
npm install typescript --save-dev
```

- To compile:

```sh
tsc filename.ts
```

- To run, once compiled to JavaScript:

```sh
node filename.js
```

Example:

```typescript
let message: string = 'Hello, TypeScript!';
console.log(message);
```
