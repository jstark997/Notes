# Modules

In NestJS, **modules** play a crucial role in organizing code and managing dependencies. A module is a class annotated with the `@Module()` decorator, and it acts as a container for closely related components like controllers, services, and providers. In addition to code organization, modules are also the key mechanism by which **dependency injection (DI)** is implemented in NestJS.

#### What is Dependency Injection?

Dependency Injection is a design pattern where one object supplies the dependencies of another object. Instead of a class creating its own dependencies, they are provided (or "injected") into the class, typically through the constructor. This makes the code more modular, testable, and maintainable.

#### How NestJS Implements Dependency Injection

NestJS uses **dependency injection** to manage how different parts of the application, such as services, are created and provided to other parts (e.g., controllers). By leveraging TypeScript's type system and decorators, NestJS can automatically resolve and inject dependencies without requiring manual wiring of objects. Dependencies are typically provided through **providers** like services, repositories, or helpers.

- **Providers** are classes annotated with the `@Injectable()` decorator.
- **Controllers** or other services can declare their dependencies in the constructor, and NestJS will inject those dependencies automatically.

#### How Modules Relate to Dependency Injection

Each module defines a context for dependency injection. When you declare a provider (like a service) in a module, that provider can be injected into any controller or other provider that is part of the same module. Additionally, modules can "export" their providers, making them available to other modules that import them.

#### Basic Example of Dependency Injection Within a Module

Let’s take a simple example where we have a `UsersModule`, a service (`UsersService`), and a controller (`UsersController`). The controller will depend on the service, and NestJS will inject the service automatically.

```typescript
// users.service.ts
import { Injectable } from "@nestjs/common";

@Injectable()
export class UsersService {
  findAll() {
    return ["User1", "User2", "User3"];
  }
}
```

The `@Injectable()` decorator marks `UsersService` as a provider that can be injected. Now, we can inject this service into a controller.

```typescript
// users.controller.ts
import { Controller, Get } from "@nestjs/common";
import { UsersService } from "./users.service";

@Controller("users")
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  findAll() {
    return this.usersService.findAll();
  }
}
```

Here, `UsersController` has a dependency on `UsersService`, which is injected via the constructor. When the `findAll()` method is called, the controller delegates to the service, which handles the logic.

Now, in the `UsersModule`, we tie everything together:

```typescript
// users.module.ts
import { Module } from "@nestjs/common";
import { UsersController } from "./users.controller";
import { UsersService } from "./users.service";

@Module({
  controllers: [UsersController],
  providers: [UsersService],
})
export class UsersModule {}
```

- `providers: [UsersService]` makes `UsersService` available within the `UsersModule`.
- `UsersController` can then inject and use `UsersService` because it's part of the same module.

#### Example of Dependency Injection Between Modules

Modules can also share providers between them. This is done by **exporting** the providers from one module and **importing** them in another module.

For instance, let’s say we want to have a `ProductsModule` that depends on services from the `UsersModule`.

1. **First, export the `UsersService` from the `UsersModule`**:

   ```typescript
   // users.module.ts
   import { Module } from "@nestjs/common";
   import { UsersService } from "./users.service";
   import { UsersController } from "./users.controller";

   @Module({
     controllers: [UsersController],
     providers: [UsersService],
     exports: [UsersService], // Export UsersService to make it available to other modules
   })
   export class UsersModule {}
   ```

2. **Import the `UsersModule` in `ProductsModule`** and inject `UsersService`:

   ```typescript
   // products.module.ts
   import { Module } from "@nestjs/common";
   import { ProductsService } from "./products.service";
   import { ProductsController } from "./products.controller";
   import { UsersModule } from "../users/users.module";

   @Module({
     imports: [UsersModule], // Import UsersModule to access its exported providers
     controllers: [ProductsController],
     providers: [ProductsService],
   })
   export class ProductsModule {}
   ```

3. **Inject `UsersService` into `ProductsService`**:

   ```typescript
   // products.service.ts
   import { Injectable } from "@nestjs/common";
   import { UsersService } from "../users/users.service";

   @Injectable()
   export class ProductsService {
     constructor(private readonly usersService: UsersService) {}

     getUsersAndProducts() {
       const users = this.usersService.findAll(); // Use UsersService from another module
       return {
         users,
         products: ["Product1", "Product2"],
       };
     }
   }
   ```

4. **Controller Example**:

   ```typescript
   // products.controller.ts
   import { Controller, Get } from "@nestjs/common";
   import { ProductsService } from "./products.service";

   @Controller("products")
   export class ProductsController {
     constructor(private readonly productsService: ProductsService) {}

     @Get()
     getUsersAndProducts() {
       return this.productsService.getUsersAndProducts();
     }
   }
   ```

In this example, `ProductsService` is able to inject `UsersService` from `UsersModule` because the `UsersService` is **exported** by `UsersModule` and **imported** into `ProductsModule`. This way, the `ProductsModule` can access shared dependencies from the `UsersModule`.

#### Summary

- **Modules** in NestJS help group related code and serve as boundaries for dependency injection.
- Providers (such as services) defined within a module can be injected into controllers or other providers within that same module.
- **Dependency Injection** allows for loose coupling of components and makes the application more testable.
- To share providers between modules, **export** them in one module and **import** the module in another.
- This modular and injectable structure makes NestJS a powerful and scalable framework for building enterprise-level applications.
