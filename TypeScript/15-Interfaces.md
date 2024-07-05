# TypeScript - Interfaces

Interfaces define the shape of an object.

Example:

```typescript
interface Person {
  name: string;
  age: number;
}

let user: Person = { name: 'Alice', age: 25 };
```

Interfaces can be defined piecewise.

Exmaple:

```typescript
interface User {
  name: string;
  login: string;
}

interface User {
  age: number;
  role: string;
}
```

In the above the properties of the User interface are defined separately.

Interfaces are commonly used to define the properties a class must implement.

```typescript
interface Credentials {
  password: string;
  email: string;
}

class AuthCredentials implements Credentials {
  email: string;
  password: string;
  userName: string;
}

function login(credentials: Credentials) {
  ...
}
```
