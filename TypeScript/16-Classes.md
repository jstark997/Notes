# TypeScript - Classes

Classes are blueprints for creating objects with properties and methods.

Example:

```typescript
class Greeter {
  greeting: string;

  constructor(message: string) {
    this.greeting = message;
  }

  greet() {
    return `Hello, ${this.greeting}`;
  }
}

let greeter = new Greeter('world');
console.log(greeter.greet());
```
