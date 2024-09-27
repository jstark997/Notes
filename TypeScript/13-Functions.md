# TypeScript - Functions

Functions can be named or anonymous, and can have typed parameters and return types.

## Named

```typescript
function greet(name: string): string {
  return `Hello, ${name}`;
}
```

Advantages:

- Named functions are hoisted, meaning they can be called before they are defined in the code.
- Easier to debug since the function name appears in stack traces.

Disadvantages:

- Requires a unique name within the same scope.

## Anonymous

```typescript
let greetAnonymous = function (name: string): string {
  return `Hello, ${name}`;
};
```

Advantages:

- Can be used as inline functions or callbacks.
- Provides flexibility in assigning to different variables or properties.

Disadvantages:

- Not hoisted, so they must be defined before they are called.
- Harder to debug since they do not have a name.

## Arrow

```typescript
let greetArrow = (name: string): string => `Hello, ${name}`;
```

Advantages:

- Shorter and more concise syntax.
- Lexical scoping of this, making it easier to work with in nested functions and callbacks.

Disadvantages:

- Not hoisted, so they must be defined before they are called.
- Cannot be used as constructors.
- May be less readable for those unfamiliar with the syntax.

## Function Types

Functions are values that have types like objects have types.

Exammple:

```typescript
function calculate(
  a: number,
  b: number,
  calcFn: (a: number, b: number) => number
) {
  return calcFn(a, b);
}
```
