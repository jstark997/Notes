# Custom Parameter Decorator

In NestJS, a custom parameter decorator can be created by using the `createParamDecorator` function provided by the framework. Here's a step-by-step guide on how to create a custom parameter decorator:

### Example: Creating a `User` Decorator

Let's say you want to create a custom decorator to extract user information from the request object. Here's how you can do it:

#### Step 1: Create the custom decorator

First, create a new file for the decorator (e.g., `user.decorator.ts`).

```typescript
import { createParamDecorator, ExecutionContext } from "@nestjs/common";

export const User = createParamDecorator(
  (data: unknown, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    return request.user; // Assuming the request has a user object (e.g., from authentication middleware)
  }
);
```

#### Step 2: Use the custom decorator in a controller

Now that the `User` decorator is created, you can use it in any controller to easily access the user from the request.

```typescript
import { Controller, Get } from "@nestjs/common";
import { User } from "./user.decorator"; // Import the custom decorator

@Controller("profile")
export class ProfileController {
  @Get()
  getProfile(@User() user: any) {
    return user; // This will return the user object from the request
  }
}
```

#### Step 3: Authentication Middleware/Guard (optional)

In real applications, you may have authentication logic (middleware, guards, or interceptors) that adds a `user` object to the request. For instance, you could be using JWT authentication, and after the token is validated, the user is attached to the request object.

#### Step 4: Handling Optional Parameters

You can pass an optional parameter to the custom decorator to extract specific properties from the `user` object:

```typescript
export const User = createParamDecorator(
  (data: string, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    const user = request.user;
    return data ? user?.[data] : user; // Return a specific property if requested
  },
);

// Example usage in a controller:
@Get()
getProfile(@User('email') userEmail: string) {
  return userEmail; // This will return the user's email from the request object
}
```

### Summary

This example demonstrates how to create a custom parameter decorator in NestJS using `createParamDecorator`, allowing you to easily extract data from the request (such as a user object). You can customize it further based on the needs of your application.

Let me know if you need further details on this!
