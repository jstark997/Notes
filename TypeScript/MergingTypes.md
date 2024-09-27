# Merging Types

Types can be merged either by using type aliases or interfaces.

## Merging Type Aliases

Type aliases can be merged into another alias with the `&` operator.

Example:

```typescript
type Admin = {
  permissions: string[];
};

type User = {
  userName: string;
};

type AdminUser = Admin & User;

let admin: AdminUser;

admin = {
  permissions: ['login', 'system'],
  userName: 'John',
};
```

## Merging Interfaces

Interfaces can be merged with the `extends` keyword.

Example:

```typescript
interface Admin {
  permissions: string[];
}

interface User {
  userName: string;
}

interface AdminUser extends Admin, User {}

let admin: AdminUser;

admin = {
  permissions: ['login', 'system'],
  userName: 'John',
};
```

**Note** - The AdminUser interface above can add other properties not in either the Amdin or User interfaces.
