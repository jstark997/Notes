# TypeScript - Namespaces

Namespaces are a way to organize code in a logical manner.

Example:

```typescript
namespace MyNamespace {
  export class MyClass {
    name: string;

    constructor(name: string) {
      this.name = name;
    }
  }
}

let myClassInstance = new MyNamespace.MyClass('Instance');
console.log(myClassInstance.name);
```
