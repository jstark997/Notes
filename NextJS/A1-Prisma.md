# Prisma

## Description

Prisma is an open-source ORM (Object-Relational Mapping) tool that makes it easier to work with databases in a type-safe manner.

1. Schema Definition:
   Prisma allows you to define your database schema using a schema file (schema.prisma). This file includes the definition of models that represent the tables in your database.

```prisma
model User {
  id    Int     @id @default(autoincrement())
  name  String
  email String  @unique
}
```

2. Migration Management:
   Prisma provides migration tools to apply changes to your database schema. You can create migrations, track changes, and ensure your database schema stays in sync with your Prisma schema.

3. Type-Safe Queries:
   Prisma generates a client library based on your schema, which you can use to perform database operations in a type-safe manner. This means you get autocompletion and type-checking benefits within your code editor.

4. Integration with Next.js API Routes:
   In a Next.js application, Prisma can be used within API routes to interact with your database. This makes it straightforward to perform CRUD (Create, Read, Update, Delete) operations as part of your API endpoints.

5. Database Agnostic:
   Prisma works with multiple databases, including PostgreSQL, MySQL, SQLite, and SQL Server. This makes it a flexible choice if you decide to switch databases in the future.

6. Real-Time and Batch Operations:
   Prisma supports real-time updates and batch operations, making it suitable for complex applications that require these features.

7. Developer Experience:
   Prisma’s integration with TypeScript enhances the developer experience by providing auto-completion, error-checking, and other IDE features. This reduces the likelihood of runtime errors and improves code maintainability.

## Installation

```sh
npm install prisma
```

## Initilization

Example using SQLite database as the datasource

```sh
npx prisma init --datasource-provider sqlite
```

After initialization there will be a new prisma directory in the project directory containing a schema.prisma file.

```
- my-app/
  - .next/
  - node_modules/
  - prisma/
    - schema.prisma
  - public/
  - src/
    - app/
```

It is in the schema.prisma file that database schemas are defined.

## Migration

When schemas are added or changed, Prisma needs to migrate the database to reflect the schema changes.

```sh
npx prisma migrate dev
```

In the above Prisma is being told to migrate to a database called 'dev'.

## Creating A Prisma Client

1. Create a directory in the src directory with an index.ts file to contain the client code.

```
- my-app/
  - .next/
  - node_modules/
  - prisma/
    - schema.prisma
  - public/
  - src/
    - db/
      - index.ts
    - app/
```

2. In index.ts create the Prisma client.

```typescript
import { PrismaClient } from '@prisma/client';

export const db = new PrismaClient();
```

## Import

If the Prisma client is created in a directory called db and in a file called index.ts then it can be imported as follows:

```typescript
import { db } from '@db';
```

## Usage

Given a schema as follows:

```prisma
model Snippet {
  id    Int    @id @default(autoincrement())
  title String
  code  String
}
```

Here is an example of creating a Snippet record in the database:

```typescript
db.snippet.create({
  data: {
    title: 'ABC function',
    code: 'const abc = () => {}',
  },
});
```

## Schema Interfaces

Prisma automatically defines interfaces for the schemas that can be imported into components and used for type checking.

Example - Import Prisma defined interface for schema:

```typescript
import type { Snippet } from '@prisma/client';
```
