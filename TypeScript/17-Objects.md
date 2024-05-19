# TypeScript - Objects

Objects can be created using object literals or constructor functions.

Example:

```typescript
let obj: { name: string; age: number } = { name: 'Alice', age: 25 };

class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}

let person = new Person('Bob', 30);
```
