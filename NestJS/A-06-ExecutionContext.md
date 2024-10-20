# Execution Context

In **NestJS**, an `ExecutionContext` provides a way to access metadata about the current request lifecycle, such as route handlers, class methods, and even information about the client making the request. It is a wrapper around an incoming request or event, and it helps interceptors, guards, and other middlewares get details about the execution flow.

### Where is `ExecutionContext` Used?

`ExecutionContext` is primarily used in:

- **Guards**
- **Interceptors**
- **Pipes**
- **Exception Filters**

Each of these components may need to retrieve information about the current request (or event), the handler being executed, or the class that contains the handler. The `ExecutionContext` gives access to these details.

### Key Methods of `ExecutionContext`

- **`getClass()`**: Returns the class that contains the route handler being called.
- **`getHandler()`**: Returns the method handler being called.
- **`getArgs()`**: Returns an array of arguments passed to the handler. For HTTP requests, these are the request, response, and `next` object.
- **`getArgByIndex(index: number)`**: Returns a specific argument from the arguments array by index.
- **`getType()`**: Returns the type of the request (e.g., `http`, `rpc`, `ws`).
- **`switchToHttp()`**: Converts the execution context to an HTTP context and allows retrieving HTTP-specific arguments like the `request` and `response`.

### Example: Using `ExecutionContext` in a Guard

Here's an example of how `ExecutionContext` is used in a **guard** to enforce authorization based on the current user.

```typescript
import { Injectable, CanActivate, ExecutionContext } from "@nestjs/common";
import { Observable } from "rxjs";

@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private requiredRole: string) {}

  canActivate(
    context: ExecutionContext
  ): boolean | Promise<boolean> | Observable<boolean> {
    // Convert the context to an HTTP context and extract the request
    const request = context.switchToHttp().getRequest();
    const user = request.user;

    // Check if the user has the required role
    return user && user.role === this.requiredRole;
  }
}
```

In this example:

- The `ExecutionContext` is converted to an HTTP context with `switchToHttp()`, which allows access to the `request`.
- The `request.user` object is checked to ensure the user has the required role.

### Example: Using `ExecutionContext` in an Interceptor

You can also use `ExecutionContext` in an **interceptor** to log the request handler being invoked:

```typescript
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
} from "@nestjs/common";
import { Observable } from "rxjs";
import { tap } from "rxjs/operators";

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const handler = context.getHandler();
    const className = context.getClass().name;

    console.log(`Before: Handling ${className}#${handler.name}`);

    return next
      .handle()
      .pipe(
        tap(() =>
          console.log(`After: ${className}#${handler.name} has finished`)
        )
      );
  }
}
```

In this case:

- The `ExecutionContext` is used to log the name of the handler and the class before and after the handler is invoked.

### Example: Customizing ExecutionContext for WebSocket

NestJS supports different types of applications, and the `ExecutionContext` adapts to them. For example, when dealing with WebSocket contexts, you can switch to the WebSocket context like this:

```typescript
const client = context.switchToWs().getClient();
const data = context.switchToWs().getData();
```

Here, `switchToWs()` retrieves WebSocket-specific information such as the client connection and any message data being transmitted.

### Summary

- `ExecutionContext` is a powerful utility in NestJS that gives access to various metadata about the request lifecycle, regardless of whether it's an HTTP request, WebSocket connection, or other transport mechanisms.
- It is most commonly used in guards, interceptors, and other lifecycle handlers to get information about the current request or event.
- By using methods like `getClass()`, `getHandler()`, and `switchToHttp()`, you can easily inspect the context of the request and customize the behavior of your application.

This flexibility makes `ExecutionContext` invaluable for adding custom logic to your NestJS applications.
