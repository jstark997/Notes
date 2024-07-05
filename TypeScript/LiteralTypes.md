# Literal Types

Variables can be annotated with types that specify a particular value or union of values, a literal type, instead of just a type of values.

Example:

```typescript
let role: 'admin' | 'user' | 'editor';

role = 'user';
```

In the above the role variable is annotated with a union of string literals as a type. The role variable can only be assigned to one of the literals in the union and no other value.
