# TypeORM Query Builder

The **TypeORM Query Builder** is a powerful and flexible tool in TypeORM that allows you to construct SQL queries programmatically using a chainable API. It's especially useful for building complex queries dynamically, with conditions that depend on runtime parameters. It allows you to easily define `SELECT`, `JOIN`, `WHERE`, `ORDER BY`, `LIMIT`, and more, all while benefiting from TypeScript's type safety.

### Example Scenario:

You have a function that takes an object with multiple properties, and you want to construct a query dynamically using these properties. This example will use a `User` entity and query for users based on their `firstName` and `age`, while also sorting the results by `createdAt` and limiting the number of results.

### Code Example:

```typescript
import { Injectable } from "@nestjs/common";
import { Repository } from "typeorm";
import { InjectRepository } from "@nestjs/typeorm";
import { User } from "./user.entity";

interface QueryParams {
  firstName?: string;
  age?: number;
  limit?: number;
  sortOrder?: "ASC" | "DESC";
}

@Injectable()
export class UserService {
  constructor(
    @InjectRepository(User)
    private userRepository: Repository<User>
  ) {}

  async findUsers(params: QueryParams): Promise<User[]> {
    const { firstName, age, limit = 10, sortOrder = "ASC" } = params;

    const queryBuilder = this.userRepository
      .createQueryBuilder("user")
      .where("user.firstName = :firstName", { firstName }) // first condition using 'where'
      .andWhere("user.age = :age", { age }) // subsequent condition using 'andWhere'
      .orderBy("user.createdAt", sortOrder) // ordering by createdAt
      .limit(limit); // limiting the number of results

    // Execute the query and return the results
    return await queryBuilder.getMany();
  }
}
```

### Breakdown:

1. **Function Parameters:**

   The function `findUsers` accepts an object `params` of type `QueryParams`. The `QueryParams` interface allows optional filtering by `firstName`, `age`, and allows specifying `limit` and `sortOrder` for query customization.

2. **Dynamic Query Building:**

   - A `WHERE` clause is added to filter by `firstName`.
   - Another condition to filter by `age` is added using the `andWhere` method.

3. **Order and Limit:**

   - The `orderBy` clause sorts the result by `createdAt`, with the sort order being dynamic (`ASC` or `DESC`) based on the provided `sortOrder`.
   - The `limit` clause restricts the number of results returned, defaulting to 10 if not provided.

4. **Query Execution:**
   - The `getMany()` method executes the query and returns an array of `User` entities.

### Usage Example:

```typescript
const users = await this.userService.findUsers({
  firstName: "John",
  age: 30,
  limit: 5,
  sortOrder: "DESC",
});
```

This would return up to 5 users named 'John' who are 30 years old, sorted by `createdAt` in descending order.

This approach gives you the flexibility to add more complex conditions or joins if needed, all while leveraging TypeORM’s query builder to dynamically construct the SQL query.
