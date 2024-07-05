# TypeScript - Types

TypeScript provides several types:

- `boolean`: true or false
- `number`: integers and floating-point numbers
- `string`: text data
- `array`: list of values
- `tuple`: fixed-size array with known types
- `enum`: set of named constants
- `any`: any type, opt-out of type checking
- `void`: no type, usually for functions that do not return a value
- `null` and `undefined`: absence of value
- `never`: values that never occur

Example:

```typescript
let isDone: boolean = false;
let decimal: number = 6;
let color: string = 'blue';
```

Example - Type inference:

```typescript
let name;

name = 'John';
```

In the above TypeScript infers that the variable 'name' is of type string because a string value was assigned to it. However, if a number had been assigned to the variable then TypeScript would infer that the variable is of type number.

Example - Explicit type annotation:

```typescript
let name: string;

name = 'John';
```

In the above the variable 'name' is annotated with the type string. Therefore TypeScript will only allow string values to be assigned to the variable.
