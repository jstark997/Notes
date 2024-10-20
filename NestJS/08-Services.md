# Services

In NestJS, **services** are classes responsible for handling business logic, interacting with data sources (such as databases or APIs), and managing other operations such as sending emails or handling background tasks. The primary purpose of services is to separate the business logic from the request handling logic (which is handled by controllers). This separation of concerns promotes cleaner, more maintainable, and testable code.

#### Primary Purpose of Services

- **Business Logic**: Services encapsulate the core business logic of the application. By isolating this logic within services, controllers remain lean and focused on handling HTTP requests and responses.

- **Reusability**: Services can be reused across different parts of the application, including multiple controllers or other services.

- **Dependency Injection**: Services in NestJS are usually decorated with the `@Injectable()` decorator, which makes them eligible for dependency injection. This means they can be injected into controllers or other services, allowing for better modularity and flexibility.

- **Separation of Concerns**: Services help keep controllers focused on managing HTTP requests, while the core business operations (e.g., user authentication, database operations, etc.) are handled by services.

#### Defining a Service

To create a service in NestJS, you annotate a class with the `@Injectable()` decorator. This allows the service to be injected into other parts of the application (e.g., controllers or other services).

Here’s an example of a simple service:

```typescript
import { Injectable } from "@nestjs/common";

@Injectable()
export class UsersService {
  private users = [
    { id: 1, name: "John Doe", email: "john@example.com" },
    { id: 2, name: "Jane Doe", email: "jane@example.com" },
  ];

  // Business logic: Retrieve all users
  findAll() {
    return this.users;
  }

  // Business logic: Find user by ID
  findOne(id: number) {
    return this.users.find((user) => user.id === id);
  }
}
```

In this example:

- The `@Injectable()` decorator marks the `UsersService` class as a service that can be injected into other parts of the application.
- The service contains two methods, `findAll()` to retrieve all users and `findOne(id)` to retrieve a specific user by ID.

#### Using Services in Controllers

Once a service is defined, it can be injected into a controller using **constructor injection**. This allows the controller to delegate the business logic to the service while focusing on handling HTTP requests.

Example of using a service within a controller:

```typescript
import { Controller, Get, Param } from "@nestjs/common";
import { UsersService } from "./users.service";

@Controller("users")
export class UsersController {
  // Injecting the UsersService
  constructor(private readonly usersService: UsersService) {}

  @Get()
  findAll() {
    // Delegate the logic to the service
    return this.usersService.findAll();
  }

  @Get(":id")
  findOne(@Param("id") id: string) {
    const userId = parseInt(id, 10); // Convert string to number
    return this.usersService.findOne(userId);
  }
}
```

In this example:

- The `UsersController` depends on the `UsersService`, and it receives it via constructor injection.
- The `findAll()` and `findOne()` methods delegate their business logic to the service, making the controller’s code cleaner and more focused on routing.

#### Services as Providers

In NestJS, **services are considered providers**. Providers are classes that can be injected and used across the application. To make a service available for injection, you must declare it as a provider within a module.

Example of registering a service in a module:

```typescript
import { Module } from "@nestjs/common";
import { UsersController } from "./users.controller";
import { UsersService } from "./users.service";

@Module({
  controllers: [UsersController],
  providers: [UsersService], // Register UsersService as a provider
})
export class UsersModule {}
```

In this example, the `UsersService` is registered as a provider in the `UsersModule`. This means that it can be injected into any other part of the `UsersModule`, such as controllers or other services.

#### Services and Dependency Injection

NestJS allows services to depend on other services via dependency injection. This can be useful in cases where one service needs to use the functionality of another service.

Here’s an example where one service depends on another:

```typescript
@Injectable()
export class ProductsService {
  findAll() {
    return ["Product1", "Product2", "Product3"];
  }
}

@Injectable()
export class OrdersService {
  constructor(private readonly productsService: ProductsService) {}

  getOrderDetails() {
    const products = this.productsService.findAll();
    return {
      orderId: 123,
      products,
    };
  }
}
```

In this example:

- The `OrdersService` depends on the `ProductsService` and uses it to retrieve product data.
- The `ProductsService` is injected into the `OrdersService` via the constructor.

For this to work, both services need to be registered as providers in the module:

```typescript
@Module({
  providers: [ProductsService, OrdersService],
})
export class OrdersModule {}
```

#### Conclusion

- **Services** in NestJS encapsulate business logic, making it easier to organize, test, and maintain your code.
- They are decorated with the `@Injectable()` decorator, which makes them eligible for dependency injection.
- Services can be injected into controllers or other services, promoting reusability and separation of concerns.
- They are registered as **providers** in a module, which makes them available for use throughout that module.
- By leveraging services, you can create modular, testable, and scalable applications.
