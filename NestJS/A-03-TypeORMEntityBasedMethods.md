# Type ORM Entity Based Methods Create, Save And Remove

In TypeORM, the `create` and `save` methods are commonly used together to **initialize** and then **persist** an entity to the database. Together, they provide a structured approach to handling entity creation and saving, while also leveraging TypeORM's lifecycle hooks. These hooks enable you to perform actions automatically before or after certain database operations, such as validation, transformation, or logging.

1. **`create`**: This method initializes an entity instance based on a plain JavaScript object. It does not save the entity to the database. The `create` method ensures that the data conforms to the structure of the entity, preparing it for database insertion.
2. **`save`**: This method persists the entity to the database, either inserting it as a new record or updating an existing one, depending on whether the entity already has a primary key. It also triggers relevant **entity lifecycle hooks**, such as `beforeInsert`, `beforeUpdate`, `afterInsert`, and `afterUpdate`.

#### Why Use `create` and `save` Together?

The typical use case for using `create` and `save` together is when you need to ensure that:

- The incoming data is properly converted into an entity instance before it is saved.
- The entity follows any pre-defined entity lifecycle rules, such as validations or transformations, through hooks.

#### Entity Lifecycle Hooks

TypeORM provides **entity lifecycle hooks** (also known as **listeners**) that are triggered automatically before or after specific actions on an entity. These hooks enable you to intercept entity events and execute logic at certain stages of the entity’s lifecycle, such as before inserting, updating, or removing data from the database.

Common hooks include:

- **`beforeInsert`**: Triggered before a new entity is inserted into the database.
- **`afterInsert`**: Triggered after a new entity has been inserted.
- **`beforeUpdate`**: Triggered before an existing entity is updated.
- **`afterUpdate`**: Triggered after an existing entity has been updated.
- **`beforeRemove`**: Triggered before an entity is removed from the database.
- **`afterRemove`**: Triggered after an entity has been removed.

#### Example: Using `create`, `save`, and Lifecycle Hooks

Let’s consider an example of a `User` entity where we want to hash the user's password before inserting or updating it in the database. We’ll use hooks to achieve this.

```typescript
import {
  Entity,
  PrimaryGeneratedColumn,
  Column,
  BeforeInsert,
  BeforeUpdate,
} from "typeorm";
import * as bcrypt from "bcrypt";

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column()
  email: string;

  @Column()
  password: string;

  // Hook to hash the password before inserting or updating the entity
  @BeforeInsert()
  @BeforeUpdate()
  async hashPassword() {
    this.password = await bcrypt.hash(this.password, 10);
  }
}
```

In this example:

- **`BeforeInsert`** and **`BeforeUpdate`** hooks ensure that the password is hashed **before** the entity is inserted or updated in the database. This ensures security and consistency in the data.

Now, let’s see how `create` and `save` work with this entity:

```typescript
async createUser(userData: Partial<User>): Promise<User> {
  // Step 1: Create a new entity instance from the input data
  const newUser = this.usersRepository.create(userData);

  // Step 2: Persist the newly created entity in the database
  // The `BeforeInsert` hook will be triggered here, hashing the password
  return this.usersRepository.save(newUser);
}
```

When `save` is called:

- The `BeforeInsert` hook is triggered, hashing the password before the user is saved to the database.
- After the insertion, the `afterInsert` hook (if defined) would also be triggered.

#### Contrast with `insert` and `update` Methods

The `insert` and `update` methods are **lower-level** operations that directly modify the database without involving entity lifecycle hooks or loading the full entity into memory.

- **`insert`**: Directly inserts a new row into the database without creating an entity instance and without triggering any hooks. It’s faster, but doesn’t provide the advantages of the entity lifecycle or relational integrity checks.

- **`update`**: Directly updates columns in the database without loading the full entity and does not trigger hooks like `beforeUpdate` or `afterUpdate`.

#### Example: Using `insert` and `update`

1. **Using `insert`**:

```typescript
async insertUser(userData: Partial<User>): Promise<void> {
  await this.usersRepository.insert(userData);  // Does not trigger hooks
}
```

The `insert` method directly inserts data into the database. It skips the entity lifecycle, so the `beforeInsert` or `beforeUpdate` hooks would **not** be triggered in this case. You should use `insert` when you want to avoid the overhead of entity lifecycle management.

2. **Using `update`**:

```typescript
async updateUser(id: number, updateData: Partial<User>): Promise<void> {
  await this.usersRepository.update(id, updateData);  // Does not trigger hooks
}
```

Similarly, `update` updates fields directly in the database without triggering the `beforeUpdate` or `afterUpdate` hooks. This method is used when you want to update fields without loading the entity into memory.

#### Key Differences Between `create`/`save` and `insert`/`update`:

- **`create` + `save`**:
  - Used together to convert raw data into an entity and persist it.
  - Triggers lifecycle hooks like `beforeInsert`, `beforeUpdate`, `afterInsert`, and `afterUpdate`.
  - Suitable for situations where you need entity lifecycle events or relationship management.
- **`insert` and `update`**:
  - Bypass the entity lifecycle and directly modify the database.
  - Do not trigger any entity hooks.
  - Faster and more efficient for simple insertions or updates where hooks are not needed.

### The `remove` and `delete` Methods

In TypeORM, both the `remove` and `delete` methods are used to delete records from the database, but they operate in different ways and trigger different lifecycle events.

#### `remove` Method

The `remove` method operates on **entity instances**. It first loads the entity from the database, then deletes it, and triggers lifecycle hooks like `beforeRemove` and `afterRemove`.

- **Entity-Based**: Requires loading the entity into memory.
- **Lifecycle Hooks**: Triggers hooks like `beforeRemove` and `afterRemove`.
- **Cascading Deletes**: If cascading relations are defined (e.g., `cascade: true`), related entities are also removed.

#### Example: Using `remove`

```typescript
async removeUser(id: number): Promise<void> {
  const user = await this.usersRepository.findOneBy({ id });
  if (user) {
    await this.usersRepository.remove(user);  // Triggers hooks
  }
}
```

In this case:

- The entity is loaded first using `findOneBy`.
- The `remove` method is then called on the entity, which triggers the `beforeRemove` and `afterRemove` hooks, and any cascading delete operations.

#### `delete` Method

The `delete` method operates directly on the database and does **not require loading the entity**. It deletes the row based on a primary key or condition but does not trigger lifecycle hooks like `beforeRemove` or `afterRemove`.

- **Criteria-Based**: Works directly with raw criteria (like an ID).
- **No Hooks**: Does not trigger any entity lifecycle events.
- **No Cascade**: Cascading deletes are not handled unless explicitly defined at the database level.

#### Example: Using `delete`

```typescript
async deleteUser(id: number): Promise<void> {
  await this.usersRepository.delete(id);  // Does not trigger hooks
}
```

In this case:

- The `delete` method directly removes the row in the database that matches the given `id`, but it bypasses the entity loading process and does not trigger any lifecycle events.

#### Key Differences Between `remove` and `delete`:

- **`remove`**:
  - Works with entity instances, triggering lifecycle hooks (`beforeRemove`, `afterRemove`).
  - Useful for scenarios where you want cascading deletes or need to execute business logic before or after removing an entity.
  - Slower because it requires loading the entity first.
- **`delete`**:
  - Operates directly on database records by criteria (e.g., IDs) and bypasses the entity lifecycle.
  - Faster because it avoids loading the entity and doesn’t trigger lifecycle events.
  - Suitable when you just need to delete a record quickly without managing cascading or related entities.

#### Hooks in `remove` and `delete`

- **`remove`** triggers the `beforeRemove` and `afterRemove` hooks, allowing you to perform actions before or after the entity is deleted.
- **`delete`** bypasses these hooks, so no additional actions are performed unless explicitly coded elsewhere.

### Summary

- **`create` + `save`**: These methods are used together to initialize a new entity instance (`create`) and persist it to the database (`save`). Lifecycle hooks like `beforeInsert`, `beforeUpdate`, `afterInsert`, and `afterUpdate` can be used to add business logic before or after these actions.
- **`insert` and `update`**: These
