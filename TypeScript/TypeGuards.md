# Type Guards

It is common to use conditionals to check the type value of a union type, as a guard, before executing any code.

Example:

```typescript
tyep Role = 'admin' | 'user' | 'editor';

let role: Role;

function performAction(action: string | number, role: Role) {
  if(typeof action === 'string') {
    if (role === 'admin') {
      // perform the action ...
    }
  } else {
    // do something else ...
  }
}
```

The `typeof`, `instanceof` and `in` operators are all used in type guards.

**Note** - It is not possible to check if a value meets the defintion of a custom type, as opposed to a built-in type or type literal, because there is no JavaScript equivalent for doing that.

TypeScript is able to infer that a variable is a specific type after the condition of a type guard has been met, in a what is called **type narrowing**.

Example:

```typescript
function combine(a: number | string, b: number | string) {
  if (typeof a === 'number' && typeof b === 'number') {
    return a + b;
  }
  return `${a}${b}`;
}
```

In the above if both a and b are of type number, then TypeScript will narrow the types of a and b to number and let the mathematical addition operation execute. However, if the condition of the if statement is not met then the types of a and b will remain as the union type string | number.
