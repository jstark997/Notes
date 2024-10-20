# Middleware

In the context of NestJS, middleware is a function that can intercept requests before they reach the route handler and can also manipulate the request and response objects. Middleware operates between the client request and the server's handling of that request. It's typically used for tasks like logging, authentication, validation, and request transformations.

### Creating Custom Middleware in NestJS

To create custom middleware, you need to implement the `NestMiddleware` interface, which forces you to define a method `use(req, res, next)`. The `next` function must be called at the end of the middleware function to pass control to the next middleware in the chain.

Here’s an example of creating custom middleware:

1. **Create Middleware**:

```typescript
import { Injectable, NestMiddleware } from "@nestjs/common";
import { Request, Response, NextFunction } from "express";

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log(`Request...`);
    next(); // Passes control to the next middleware
  }
}
```

2. **Apply Middleware**:

In `app.module.ts`, you can apply the middleware globally or to a specific route:

```typescript
import { Module, MiddlewareConsumer } from "@nestjs/common";
import { LoggerMiddleware } from "./middlewares/logger.middleware";
import { SomeController } from "./some/some.controller";

@Module({
  controllers: [SomeController],
})
export class AppModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(LoggerMiddleware).forRoutes(SomeController); // Applies to the SomeController routes
  }
}
```

### Middleware Execution Order

The order in which middleware is executed is determined by how it is defined in the `configure` method of the `AppModule` or in the module where it's applied. Middleware is executed in the order in which it's applied, so the first `apply` statement will run before the second, and so on.

If middleware depends on the output of another middleware, you can ensure that they are executed in the correct order by defining them in the proper sequence.

#### Example: Middleware Dependency

Let's say you have two pieces of middleware, where one depends on some transformation done by the other.

1. **Transforming Middleware** (adds a custom header):

```typescript
@Injectable()
export class HeaderMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    req.headers["x-custom-header"] = "custom-value";
    console.log("Added custom header");
    next();
  }
}
```

2. **Dependent Middleware** (logs the custom header):

```typescript
@Injectable()
export class LogCustomHeaderMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log(`Custom Header: ${req.headers["x-custom-header"]}`);
    next();
  }
}
```

3. **Ensure Execution Order**:

In the `AppModule`, ensure the `HeaderMiddleware` is applied before `LogCustomHeaderMiddleware`.

```typescript
import { Module, MiddlewareConsumer } from "@nestjs/common";
import { HeaderMiddleware } from "./middlewares/header.middleware";
import { LogCustomHeaderMiddleware } from "./middlewares/log-custom-header.middleware";
import { SomeController } from "./some/some.controller";

@Module({
  controllers: [SomeController],
})
export class AppModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(HeaderMiddleware)
      .forRoutes(SomeController) // Runs first
      .apply(LogCustomHeaderMiddleware)
      .forRoutes(SomeController); // Runs after HeaderMiddleware
  }
}
```

### Key Points:

- Middleware is executed in the order it’s applied in the `MiddlewareConsumer`.
- Ensure dependencies between middleware are respected by applying them sequentially.
