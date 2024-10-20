# Modifying A Library Interface Globally

In a NestJS application, you can modify or extend the interface of a dependency globally using TypeScript's declaration merging feature. For instance, if you want to add a `currentUser` property to the `Request` interface of the Express library, you can do so by augmenting the existing `Request` interface provided by Express.

Here's how to extend the `Request` interface globally to add a `currentUser` property:

### 1. Create a Declaration File (`*.d.ts`)

In your NestJS project, create a new file called `express.d.ts` (or any name you prefer, as long as it ends in `.d.ts`). This file is where you will augment the existing `Request` interface from the `express` package.

For example, create the file in the `src` folder:

```bash
src/express.d.ts
```

### 2. Augment the Request Interface

Inside this `.d.ts` file, you will declare a module for `express` and extend the `Request` interface.

```typescript
// src/express.d.ts

import { User } from "./path-to-your-user-entity-or-dto"; // Adjust the import to your user entity or DTO

declare global {
  namespace Express {
    interface Request {
      currentUser?: User; // Add the `currentUser` property
    }
  }
}
```

### 3. Ensure TypeScript Recognizes the Declaration File

Make sure TypeScript includes the newly created `.d.ts` file. In your `tsconfig.json` file, check that `src` is within the `include` array. Here's an example of the relevant portion of the `tsconfig.json`:

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es6",
    "strict": true
    // other compiler options...
  },
  "include": ["src/**/*"]
}
```

If `src/express.d.ts` is under the `src` directory, it will automatically be picked up by TypeScript.

### 4. Use the Extended Interface

Now that the `Request` interface is extended, you can use `currentUser` in your NestJS controllers, services, and middleware. For example, in a controller:

```typescript
import { Controller, Get, Req } from "@nestjs/common";
import { Request } from "express";

@Controller("profile")
export class ProfileController {
  @Get()
  getProfile(@Req() req: Request) {
    const currentUser = req.currentUser; // TypeScript now knows `currentUser` exists on `Request`
    return currentUser;
  }
}
```

### 5. Populate `currentUser` (e.g., in Middleware or Guards)

To populate the `currentUser` property, you can use middleware or a guard. For instance, in middleware:

```typescript
import { Injectable, NestMiddleware } from "@nestjs/common";
import { Request, Response, NextFunction } from "express";

@Injectable()
export class CurrentUserMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    // Assume you get the user from a decoded token or session
    const user = { id: 1, name: "John Doe" }; // Mock user
    req.currentUser = user;
    next();
  }
}
```

Then, apply the middleware globally or to specific routes:

```typescript
import { MiddlewareConsumer, Module, NestModule } from "@nestjs/common";
import { CurrentUserMiddleware } from "./middlewares/current-user.middleware";
import { ProfileController } from "./profile/profile.controller";

@Module({
  controllers: [ProfileController],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(CurrentUserMiddleware).forRoutes(ProfileController);
  }
}
```

### Summary

- Create a `*.d.ts` file to extend the `Request` interface globally using declaration merging.
- Ensure the `.d.ts` file is included in the TypeScript compilation.
- You can now use `currentUser` in any part of your NestJS app where the `Request` object is available.
- Populate the `currentUser` property using middleware, guards, or interceptors.

This approach is clean and leverages TypeScript's powerful declaration merging to enhance third-party interfaces globally across your application.
