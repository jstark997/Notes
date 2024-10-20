# Guards

In NestJS, a **Guard** is a fundamental feature used to control access to routes or route handlers by determining whether a particular request should be handled by the application. Guards are similar to middleware but are designed to focus specifically on **authorization logic**, such as verifying if a user has the necessary permissions or roles to access a certain resource.

Guards run before the request reaches the route handler, and they return a boolean value or a promise resolving to a boolean. If a guard returns `true`, the request proceeds; if it returns `false`, NestJS throws a `ForbiddenException` by default.

### Creating a Guard

To create a guard in NestJS, you implement the `CanActivate` interface. Here's a basic example of a guard that checks if a user is authenticated.

### Example: Auth Guard

```typescript
import { Injectable, CanActivate, ExecutionContext } from "@nestjs/common";
import { Observable } from "rxjs";

@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(
    context: ExecutionContext
  ): boolean | Promise<boolean> | Observable<boolean> {
    const request = context.switchToHttp().getRequest();
    return this.validateRequest(request);
  }

  private validateRequest(request): boolean {
    // Example: Check if a user is authenticated by checking for a token or session
    return !!request.headers.authorization; // For example, check if there's an Authorization header
  }
}
```

In this example:

- The `AuthGuard` implements the `CanActivate` interface.
- The `canActivate()` method checks if the request contains an `Authorization` header.
- If the header exists, the guard returns `true`, allowing access. If not, it returns `false`, blocking the request.

### Applying the Guard to a Route

You can apply the guard globally, to a controller, or to individual routes.

#### Applying the Guard to a Route

You can apply a guard to a specific route using the `@UseGuards()` decorator:

```typescript
import { Controller, Get, UseGuards } from "@nestjs/common";

@Controller("protected")
export class ProtectedController {
  @Get()
  @UseGuards(AuthGuard) // Applying the guard to this route
  getProtectedData() {
    return { message: "This is protected data" };
  }
}
```

#### Applying the Guard Globally

You can also apply the guard globally in the `main.ts` file:

```typescript
import { NestFactory } from "@nestjs/core";
import { AppModule } from "./app.module";
import { AuthGuard } from "./auth.guard";

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalGuards(new AuthGuard());
  await app.listen(3000);
}
bootstrap();
```

With this setup, the `AuthGuard` will be applied to every route in the application, and only requests with valid authorization headers will be allowed through.

### Summary

- Guards in NestJS are used for authorization.
- They implement the `CanActivate` interface and return a boolean value or a promise.
- Guards can be applied globally, to a controller, or to individual routes.

Would you like to explore custom guards for role-based access or other specific use cases?
