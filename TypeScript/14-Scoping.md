# TypeScript - Scoping Rules

Scoping rules in TypeScript determine the visibility and lifetime of variables and functions. There are three main types of scope: global, function, and block scope.

## Global Scope

Variables declared outside of any function or block have global scope. They are accessible from any part of the code.

```typescript
var globalVar = 'I am a global variable';

function displayGlobalVar() {
  console.log(globalVar); // Accessible
}

displayGlobalVar();
console.log(globalVar); // Accessible
```

## Function Scope

Variables declared within a function using `var` are scoped to that function. They are not accessible outside the function.

```typescript
function displayFunctionScope() {
  var functionScopedVar = 'I am function scoped';
  console.log(functionScopedVar); // Accessible within the function
}

displayFunctionScope();
console.log(functionScopedVar); // Error: functionScopedVar is not defined
```

## Block Scope

Variables declared using `let` and `const` are block-scoped, meaning they are only accessible within the block (e.g., `{}`) they are defined in.

```typescript
if (true) {
  let blockScopedVar = 'I am block scoped';
  const anotherBlockScopedVar = 'I am also block scoped';
  console.log(blockScopedVar); // Accessible within the block
  console.log(anotherBlockScopedVar); // Accessible within the block
}

console.log(blockScopedVar); // Error: blockScopedVar is not defined
console.log(anotherBlockScopedVar); // Error: anotherBlockScopedVar is not defined
```

## Variable Hoisting

`var` declarations are hoisted to the top of their scope, but not their assignments. `let` and `const` declarations are also hoisted but are not initialized.

```typescript
console.log(hoistedVar); // undefined (declaration is hoisted)
var hoistedVar = 'I am hoisted';

console.log(hoistedLet); // Error: Cannot access 'hoistedLet' before initialization
let hoistedLet = 'I am not hoisted';

console.log(hoistedConst); // Error: Cannot access 'hoistedConst' before initialization
const hoistedConst = 'I am also not hoisted';
```

## Shadowing

Variables declared in inner scopes can shadow variables with the same name in outer scopes.

```typescript
let outerVar = 'I am the outer variable';

function shadowingExample() {
  let outerVar = 'I am the inner variable';
  console.log(outerVar); // "I am the inner variable"
}

shadowingExample();
console.log(outerVar); // "I am the outer variable"
```

## Closures

Functions can capture variables from their containing scope, forming closures.

```typescript
function makeCounter() {
  let count = 0;
  return function () {
    count++;
    console.log(count);
  };
}

let counter = makeCounter();
counter(); // 1
counter(); // 2
```
