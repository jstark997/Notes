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

**Note** - A variable that is declared without a type annotation and then assigned an object as value will have type any. There is also an object type. However, it is best practice to annotate variables with the structure of the object as shown above.
