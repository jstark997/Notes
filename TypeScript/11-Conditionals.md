# TypeScript - Conditionals

Conditional statements include `if`, `else if`, `else`, and `switch`.

Example:

```typescript
let num: number = 10;
if (num > 0) {
  console.log('Positive');
} else if (num < 0) {
  console.log('Negative');
} else {
  console.log('Zero');
}

switch (num) {
  case 0:
    console.log('Zero');
    break;
  case 10:
    console.log('Ten');
    break;
  default:
    console.log('Other');
}
```
