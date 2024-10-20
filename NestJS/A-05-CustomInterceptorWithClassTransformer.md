# Custom Interceptor Using Class Transformer To Control Serialization Of Response Body

In NestJS, you can use the `class-transformer` library in combination with Data Transfer Object (DTO) classes to control how data is serialized before sending it as a response. The `class-transformer` library provides decorators like `@Exclude()` and `@Expose()` that allow you to define which properties should be included or excluded when serializing an object.

By leveraging `class-transformer` in a custom interceptor, you can cleanly manage how sensitive or unnecessary data is excluded from the response without manually deleting fields. This approach is both powerful and flexible for managing responses.

### Steps to Implement a Custom Interceptor with `class-transformer` and a DTO

1. **Install `class-transformer`**:  
   First, install the required package if it’s not already included in your project.

   ```bash
   npm install class-transformer
   ```

2. **Create a Data Transfer Object (DTO)**:  
   Define a DTO class that represents how you want to transform the data when returning it to the client. Use the `@Exclude()` decorator to specify which properties should be hidden in the response.

3. **Create a Custom Interceptor**:  
   The custom interceptor will use `class-transformer`'s `plainToClass()` function to convert your entity or response object into a serialized version using the DTO class.

4. **Apply the Interceptor to a Controller or Globally**:  
   Finally, you can apply the custom interceptor to a controller or globally in your application.

### Step-by-Step Example

#### 1. Defining a DTO with `class-transformer`

Suppose you have a `User` entity that contains sensitive fields like `password` and `internalNotes`. You don’t want these fields to be exposed in the API responses.

Define a DTO class for the `User` entity that uses `class-transformer` decorators like `@Exclude()` to omit specific fields from the response:

```typescript
// user.dto.ts
import { Exclude, Expose } from "class-transformer";

export class UserDto {
  @Expose() // This field will be included in the response
  id: number;

  @Expose()
  name: string;

  @Expose()
  email: string;

  @Exclude() // This field will be excluded from the response
  password: string;

  @Exclude() // Internal notes should not be exposed to the client
  internalNotes: string;

  // Optionally, you can also expose computed properties or rename fields
  @Expose({ name: "createdAt" }) // Exposing under a different name
  createdDate: Date;
}
```

In this example:

- **`@Exclude()`**: Used to hide properties like `password` and `internalNotes` from the API response.
- **`@Expose()`**: Explicitly specifies that certain properties should be included in the response, even if `@Exclude()` is used elsewhere.

#### 2. Creating a Custom Interceptor

Next, create a custom interceptor that uses `class-transformer`'s `plainToClass()` function to serialize the data using the `UserDto` class. This interceptor will transform the entity into a DTO before it is sent as a response.

```typescript
// transform.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
} from "@nestjs/common";
import { plainToClass } from "class-transformer";
import { Observable } from "rxjs";
import { map } from "rxjs/operators";

@Injectable()
export class TransformInterceptor implements NestInterceptor {
  constructor(private readonly dto: any) {}

  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next
      .handle()
      .pipe(
        map((data) =>
          plainToClass(this.dto, data, { excludeExtraneousValues: true })
        )
      );
  }
}
```

In this example:

- The `TransformInterceptor` takes a DTO class (`UserDto`) as a constructor argument.
- The `plainToClass()` function converts the response object into an instance of the DTO and serializes it according to the rules defined in the DTO (such as excluding fields with `@Exclude()`).
- The `excludeExtraneousValues: true` option ensures that only the properties marked with `@Expose()` in the DTO are included in the final response.

#### 3. Applying the Interceptor to a Controller

You can apply the interceptor at the controller level, or for specific routes, using the `@UseInterceptors()` decorator. In this case, we apply the interceptor to a `UsersController`.

```typescript
// users.controller.ts
import { Controller, Get, UseInterceptors } from "@nestjs/common";
import { UsersService } from "./users.service";
import { UserDto } from "./user.dto";
import { TransformInterceptor } from "./transform.interceptor";

@Controller("users")
@UseInterceptors(new TransformInterceptor(UserDto))
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  async getAllUsers() {
    // Example response from the service
    return this.usersService.findAll(); // Returns a list of users
  }
}
```

In this example:

- The `TransformInterceptor` is applied at the controller level to serialize all responses from the `UsersController` using the `UserDto`.
- The `findAll()` method in `UsersService` may return raw `User` entities, but before they reach the client, they are transformed into the `UserDto` format by the interceptor.

#### 4. Example of `UsersService`

```typescript
// users.service.ts
import { Injectable } from "@nestjs/common";
import { User } from "./user.entity";

@Injectable()
export class UsersService {
  async findAll(): Promise<User[]> {
    return [
      {
        id: 1,
        name: "John Doe",
        email: "john@example.com",
        password: "hashedpassword",
        internalNotes: "Admin user",
        createdAt: new Date(),
      },
      {
        id: 2,
        name: "Jane Doe",
        email: "jane@example.com",
        password: "hashedpassword2",
        internalNotes: "Regular user",
        createdAt: new Date(),
      },
    ];
  }
}
```

#### 5. Response Before and After Interception

**Without the Interceptor:**
The raw response from the `UsersService` may look like this:

```json
[
  {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "password": "hashedpassword",
    "internalNotes": "Admin user",
    "createdAt": "2024-10-01T12:00:00.000Z"
  },
  {
    "id": 2,
    "name": "Jane Doe",
    "email": "jane@example.com",
    "password": "hashedpassword2",
    "internalNotes": "Regular user",
    "createdAt": "2024-10-01T12:00:00.000Z"
  }
]
```

**With the Interceptor (after applying `UserDto` transformation):**
After passing through the `TransformInterceptor`, the response is serialized and sensitive fields like `password` and `internalNotes` are excluded:

```json
[
  {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "createdAt": "2024-10-01T12:00:00.000Z"
  },
  {
    "id": 2,
    "name": "Jane Doe",
    "email": "jane@example.com",
    "createdAt": "2024-10-01T12:00:00.000Z"
  }
]
```

### Summary

By combining `class-transformer` with DTOs and a custom interceptor in NestJS, you can easily control how response data is serialized and ensure sensitive fields are excluded from the API responses. This method offers a clean, reusable, and declarative approach to managing data serialization without the need for manual manipulation of response objects.

- **DTO Classes**: Define how you want the response data to look using `@Exclude()` and `@Expose()` decorators from `class-transformer`.
- **Custom Interceptors**: Use `plainToClass()` inside an interceptor to convert raw entity data into a DTO format before sending it in the response.
- **Serialization**: Ensure that sensitive or unnecessary fields are excluded from the response, improving security and ensuring a clean API design.
