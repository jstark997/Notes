# Interceptors

### What Are Interceptors in NestJS?

Interceptors in NestJS are a middleware-like feature that allows you to manipulate or transform incoming requests and outgoing responses. They enable you to apply common logic such as logging, response mapping, or error handling across different parts of the application. Interceptors sit between the request and response lifecycle, allowing you to intercept both before a request reaches the controller and after the controller processes the response.

### Common Use Cases for Interceptors

- **Transforming responses**: Modify or transform the response before sending it to the client.
- **Logging**: Log request and response data for auditing purposes.
- **Caching**: Cache responses to prevent redundant requests.
- **Error handling**: Catch and handle exceptions centrally.
- **Authorization/validation**: Check conditions or modify the request before it reaches the controller.

### How Interceptors Work

Interceptors have two main phases:

1. **Request Phase**: Code executed before the request reaches the controller.
2. **Response Phase**: Code executed after the controller has processed the request but before the response is sent to the client.

The `intercept` method of an interceptor receives two arguments:

- `ExecutionContext`: Provides methods to access details about the current request.
- `CallHandler`: Provides access to the next interceptor or the route handler.

```typescript
intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
  // Code before the request is handled
  return next.handle().pipe(
    // Code after the response is handled
  );
}
```

A custom interceptor in NestJS is created by implementing the `NestInterceptor` interface and overriding its `intercept()` method.

### Integration with Dependency Injection (DI)

Interceptors are services in NestJS and follow the standard DI pattern. You can inject services, repositories, or any other injectable components into an interceptor. Interceptors can be applied globally, at the controller level, or at specific route handlers.

### Creating a Custom Interceptor

Here is an example of creating a custom interceptor in NestJS.

#### Steps to Create an Interceptor:

1. **Generate the interceptor**: Using the Nest CLI or manually.
   ```bash
   nest g interceptor custom
   ```
2. **Define custom logic** in the `intercept` method.

```typescript
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
} from "@nestjs/common";
import { Observable } from "rxjs";
import { map } from "rxjs/operators";

@Injectable()
export class CustomInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    console.log("Before handling request...");

    // Handle the request
    return next.handle().pipe(
      // Handle the response
      map((response) => {
        console.log("After handling response...");
        // You can manipulate the response here
        return response;
      })
    );
  }
}
```

### Example of Intercepting a Request

Let’s create an example of a custom interceptor that intercepts requests and adds a custom header.

```typescript
@Injectable()
export class RequestInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const request = context.switchToHttp().getRequest();
    // Manipulate the request, e.g., add a custom header
    request.headers["x-custom-header"] = "MyCustomHeaderValue";
    console.log("Request intercepted:", request.headers);

    return next.handle();
  }
}
```

In this example, we access the request object, modify its headers, and allow the request to continue its lifecycle.

### Example of Intercepting a Response (Serialization)

A common use case is serializing or filtering out certain properties from the response before sending it to the client.

#### Example: Hiding Sensitive Fields from the Response

```typescript
@Injectable()
export class ResponseInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next.handle().pipe(
      map((response) => {
        // Modify the response object, hiding sensitive fields
        if (response && typeof response === "object") {
          delete response.password; // Hide the 'password' field
          delete response.sensitiveInfo; // Hide other sensitive fields
        }
        return response;
      })
    );
  }
}
```

In this example, after the controller returns a response, the interceptor removes sensitive fields like `password` and `sensitiveInfo` before sending the data back to the client.

### Applying an Interceptor to a Controller or Method

Interceptors can be applied globally, at the controller level, or at the method level using the `@UseInterceptors` decorator.

- **Controller or Method level**: Use the `@UseInterceptors` decorator to apply an interceptor.

```typescript
import { Controller, Get, UseInterceptors } from "@nestjs/common";

@Controller("users")
@UseInterceptors(ResponseInterceptor)
export class UserController {
  @Get()
  findAll() {
    return { username: "john_doe", password: "secret", sensitiveInfo: "xyz" };
  }
}
```

### Applying an Interceptor Globally

To apply an interceptor globally, you can register it in the main application module using `app.useGlobalInterceptors()` method. Global interceptors will be applied to every incoming request across the entire application.

#### Example of Applying an Interceptor Globally

In the `main.ts` file (or `app.module.ts`), you can register an interceptor globally like this:

```typescript
import { NestFactory } from "@nestjs/core";
import { AppModule } from "./app.module";
import { CustomInterceptor } from "./interceptors/custom.interceptor";

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Apply global interceptor
  app.useGlobalInterceptors(new CustomInterceptor());

  await app.listen(3000);
}
bootstrap();
```

In this example, the `CustomInterceptor` will be applied globally to every request made to the application. This is useful for features like logging or authentication that you want to apply across all routes.

Alternatively, you can register global interceptors in the `AppModule` using the `APP_INTERCEPTOR` token:

```typescript
import { Module } from "@nestjs/common";
import { APP_INTERCEPTOR } from "@nestjs/core";
import { CustomInterceptor } from "./interceptors/custom.interceptor";

@Module({
  providers: [
    {
      provide: APP_INTERCEPTOR,
      useClass: CustomInterceptor,
    },
  ],
})
export class AppModule {}
```

In this case, the `CustomInterceptor` is applied globally, but through the module configuration. This method can be useful when you want to use DI to inject dependencies into the interceptor.

### Example of Dependency Injection in an Interceptor

Interceptors are integrated with NestJS's DI system, so you can inject services into interceptors as needed:

```typescript
@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  constructor(private readonly loggerService: LoggerService) {}

  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    this.loggerService.log("Request intercepted");
    return next.handle();
  }
}
```

Here, `LoggerService` is injected into the interceptor via DI, allowing the interceptor to use logging functionalities from the service.

### Conclusion

Interceptors in NestJS allow for flexible handling of cross-cutting concerns such as transforming requests and responses. You can implement them globally or at a more granular level (e.g., controller or route). By leveraging NestJS’s DI system, interceptors can access other services and maintain a clean separation of concerns in your application.
