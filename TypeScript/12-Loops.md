# TypeScript - Loops

Loop constructs include `for`, `while`, `do-while`, and `for...of`.

Example:

```typescript
for (let i = 0; i < 5; i++) {
  console.log(i);
}

let count: number = 0;
while (count < 5) {
  console.log(count);
  count++;
}

count = 0;
do {
  console.log(count);
  count++;
} while (count < 5);

let array: number[] = [1, 2, 3];
for (let value of array) {
  console.log(value);
}
```
