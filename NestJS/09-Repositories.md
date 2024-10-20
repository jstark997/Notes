# Repositories

In NestJS, **repositories** play a crucial role in managing the interaction with databases, particularly when using an Object-Relational Mapping (ORM) library like TypeORM. Repositories are abstractions that encapsulate data access logic, providing methods to perform CRUD operations on entities (such as finding, saving, updating, and deleting records in the database). By separating data access logic from the business logic, repositories allow for more maintainable, modular, and testable code.

#### Relationship Between Repositories and Services

Repositories are typically used within **services** to abstract away database interactions. Services handle the business logic of the application, and they delegate the actual database operations (like fetching or saving data) to repositories. This separation allows services to focus on business rules, while repositories focus on data persistence and retrieval.

In other words:

- **Repositories** are responsible for interacting with the database.
- **Services** use repositories to fulfill the business logic and perform data operations.

#### Repositories in TypeORM

TypeORM, which is one of the supported ORMs in NestJS, provides repository classes that offer methods to manage database entities. Each entity in TypeORM is usually associated with a repository.

- **Entities**: Define the structure of a database table.
- **Repositories**: Provide methods to interact with the entity's corresponding table in the database.

You can access repositories in NestJS using the `@InjectRepository()` decorator provided by `@nestjs/typeorm`.

#### Installing TypeORM

To install TypeORM and a database driver (for example, SQLite):

```bash
npm install --save @nestjs/typeorm typeorm sqlite3
```

#### Defining an Entity

Before we work with repositories, we need to define an **entity**. An entity represents a table in the database, and each instance of an entity corresponds to a row in that table.

```typescript
// user.entity.ts
import { Entity, PrimaryGeneratedColumn, Column } from "typeorm";

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column()
  email: string;
}
```

Here, the `User` entity defines a table with three columns: `id`, `name`, and `email`.

#### Configuring the User Module with the `forFeature` Method

In order to use the `User` repository within the `UsersModule`, we need to configure the module using the `TypeOrmModule.forFeature()` method. This method registers the repository with the module, making it available for injection in the services that belong to the same module.

Here’s how to configure the `UsersModule`:

```typescript
// users.module.ts
import { Module } from "@nestjs/common";
import { TypeOrmModule } from "@nestjs/typeorm";
import { UsersService } from "./users.service";
import { UsersController } from "./users.controller";
import { User } from "./user.entity";

@Module({
  imports: [TypeOrmModule.forFeature([User])], // Register the User entity for this module
  providers: [UsersService],
  controllers: [UsersController],
})
export class UsersModule {}
```

In this example:

- `TypeOrmModule.forFeature([User])`: Registers the `User` entity, allowing the `UserRepository` to be available within the module. This is essential for injecting the repository into the `UsersService`.

#### Creating a Database Connection

Configure TypeORM in the root module (`AppModule`) to connect to the SQLite database and make the `User` entity available:

```typescript
// app.module.ts
import { Module } from "@nestjs/common";
import { TypeOrmModule } from "@nestjs/typeorm";
import { UsersModule } from "./users/users.module";
import { User } from "./users/user.entity";

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: "sqlite",
      database: "data.sqlite",
      entities: [User],
      synchronize: true, // Automatically sync database schema
    }),
    UsersModule,
  ],
})
export class AppModule {}
```

#### Defining a Repository in a Service

Once we have defined the `User` entity, registered the repository in `UsersModule` and created a database connection, we can create a service that uses the repository to perform data operations like finding and creating users.

Now, we can create the **UsersService** and inject the `UserRepository` using `@InjectRepository()`.

```typescript
// users.service.ts
import { Injectable } from "@nestjs/common";
import { InjectRepository } from "@nestjs/typeorm";
import { Repository } from "typeorm";
import { User } from "./user.entity";

@Injectable()
export class UsersService {
  constructor(
    @InjectRepository(User)
    private usersRepository: Repository<User> // Injecting the User repository
  ) {}

  // Find all users
  findAll(): Promise<User[]> {
    return this.usersRepository.find();
  }

  // Find one user by ID
  findOne(id: number): Promise<User> {
    return this.usersRepository.findOneBy({ id });
  }

  // Create a new user
  create(user: Partial<User>): Promise<User> {
    const newUser = this.usersRepository.create(user);
    return this.usersRepository.save(newUser);
  }

  // Remove a user by ID
  async remove(id: number): Promise<void> {
    await this.usersRepository.delete(id);
  }
}
```

In this example:

- We inject the `UserRepository` using `@InjectRepository(User)` into the `UsersService`.
- The `Repository<User>` is a generic repository class provided by TypeORM that provides a wide range of methods like `find()`, `save()`, and `delete()` for interacting with the `User` entity in the database.

Here are the methods in the service:

- **`findAll()`**: Returns a list of all users in the database.
- **`findOne(id)`**: Returns a user with the specified ID.
- **`create(user)`**: Creates a new user in the database and saves it.
- **`remove(id)`**: Deletes a user by its ID.

#### Using the Service in a Controller

Now, let's create a controller that interacts with the `UsersService`. This controller will handle HTTP requests and use the service to perform operations on the `User` entity via the repository.

```typescript
// users.controller.ts
import { Controller, Get, Post, Param, Body, Delete } from "@nestjs/common";
import { UsersService } from "./users.service";
import { User } from "./user.entity";

@Controller("users")
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  findAll(): Promise<User[]> {
    return this.usersService.findAll();
  }

  @Get(":id")
  findOne(@Param("id") id: string): Promise<User> {
    return this.usersService.findOne(Number(id));
  }

  @Post()
  create(@Body() createUserDto: Partial<User>): Promise<User> {
    return this.usersService.create(createUserDto);
  }

  @Delete(":id")
  remove(@Param("id") id: string): Promise<void> {
    return this.usersService.remove(Number(id));
  }
}
```

In this example:

- `findAll()` handles GET requests to `/users` and retrieves all users using the service.
- `findOne()` handles GET requests to `/users/:id` and retrieves a specific user by ID.
- `create()` handles POST requests to `/users` and creates a new user.
- `remove()` handles DELETE requests to `/users/:id` and deletes a user by ID.

#### Summary

- **Repositories** in NestJS provide an abstraction layer over the database, handling CRUD operations for entities.
- **Services** interact with repositories to delegate data persistence logic, enabling services to focus on business logic while repositories manage database interactions.
- Repositories are typically injected into services using the `@InjectRepository()` decorator.
- Services that use repositories can then be injected into controllers to handle requests, making the system modular and well-organized.

By leveraging the repository pattern in NestJS, you can maintain clear separation of concerns, improve testability, and ensure that your application is modular and scalable.
