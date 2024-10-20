# Controllers

In NestJS, **controllers** are responsible for handling incoming HTTP requests and returning responses to the client. Each controller is associated with a specific path and defines methods to handle different HTTP methods (GET, POST, PUT, DELETE, etc.). NestJS uses decorators to map routes to controller methods, making it easy to define API endpoints.

#### Defining a Basic Controller

A controller is simply a TypeScript class, decorated with the `@Controller()` decorator, which defines a base path for the routes within the controller. Each method within the class corresponds to a specific route or HTTP request.

Example of a basic controller:

```typescript
import { Controller, Get } from "@nestjs/common";

@Controller("users") // Base route: /users
export class UsersController {
  @Get() // Handles GET requests to /users
  findAll() {
    return "This action returns all users";
  }
}
```

In this example, the `@Controller('users')` decorator defines the base path for this controller as `/users`. The `@Get()` decorator defines a route for GET requests to `/users`. When a client sends a GET request to `/users`, the `findAll()` method is executed.

#### Handling Different HTTP Methods

NestJS provides decorators for handling different HTTP methods:

- `@Get()`: Handles GET requests.
- `@Post()`: Handles POST requests.
- `@Put()`: Handles PUT requests.
- `@Delete()`: Handles DELETE requests.
- `@Patch()`: Handles PATCH requests.

Example:

```typescript
@Controller("users")
export class UsersController {
  @Get() // GET request to /users
  findAll() {
    return "This action returns all users";
  }

  @Post() // POST request to /users
  create() {
    return "This action creates a new user";
  }

  @Put(":id") // PUT request to /users/:id
  update(@Param("id") id: string) {
    return `This action updates the user with ID ${id}`;
  }

  @Delete(":id") // DELETE request to /users/:id
  remove(@Param("id") id: string) {
    return `This action removes the user with ID ${id}`;
  }
}
```

In this example:

- `@Post()` handles creating a new user.
- `@Put(':id')` and `@Delete(':id')` handle updating and deleting a user by ID, using a **path parameter**.

#### Path Parameters

Path parameters allow dynamic values to be passed in the URL. To extract a path parameter from a route, use the `@Param()` decorator.

Example of a route with a path parameter:

```typescript
@Controller("users")
export class UsersController {
  @Get(":id") // GET request to /users/:id
  findOne(@Param("id") id: string) {
    return `This action returns user with ID: ${id}`;
  }
}
```

Here, the `@Get(':id')` decorator defines a route that accepts a path parameter `id`. The `@Param('id')` decorator extracts the value of the `id` parameter from the URL.

#### Query Parameters

Query parameters are key-value pairs appended to the URL. To extract query parameters, use the `@Query()` decorator.

Example with query parameters:

```typescript
@Controller("users")
export class UsersController {
  @Get()
  findAll(@Query("age") age: string, @Query("role") role: string) {
    return `This action returns users filtered by age: ${age}, role: ${role}`;
  }
}
```

In this example, when a GET request is made to `/users?age=30&role=admin`, the `findAll()` method receives the query parameters `age` and `role`.

#### Handling Data in the Request Body

For routes that handle POST or PUT requests, where data is typically sent in the request body, you can use the `@Body()` decorator to access the data.

Example:

```typescript
import { Body, Post } from "@nestjs/common";

@Controller("users")
export class UsersController {
  @Post()
  create(@Body() createUserDto: CreateUserDto) {
    return `This action creates a new user with name: ${createUserDto.name}`;
  }
}
```

Here, `createUserDto` is a Data Transfer Object (DTO) that contains the data structure for the incoming request. The `@Body()` decorator extracts the body data from the request.

A typical DTO might look like this:

```typescript
export class CreateUserDto {
  name: string;
  age: number;
}
```

#### Returning Responses and Status Codes

By default, controllers return data as JSON. However, you can customize responses, including setting status codes.

- Use the `@Res()` decorator for advanced control over the response object (but note that using `@Res()` prevents automatic response transformation).
- The `@HttpCode()` decorator allows you to specify the HTTP status code returned by a route.

Example of setting a custom status code:

```typescript
import { Controller, Post, HttpCode, Body } from "@nestjs/common";

@Controller("users")
export class UsersController {
  @Post()
  @HttpCode(201) // Return HTTP 201 Created
  create(@Body() createUserDto: CreateUserDto) {
    return `User ${createUserDto.name} created successfully`;
  }
}
```

In this example, the `@HttpCode(201)` decorator sets the HTTP status code to 201, which indicates that a resource was created.

#### Handling Errors in a Controller

In NestJS, error handling is simplified using the `HttpException` class, which allows you to throw custom exceptions with specific HTTP status codes and messages. Instead of decorators, you throw instances of the `HttpException` class (or extend it to create custom exceptions) when you want to handle errors.

NestJS also provides built-in exceptions such as `NotFoundException`, `BadRequestException`, `UnauthorizedException`, and more. These are subclasses of `HttpException` and can be used directly to simplify common error scenarios.

#### Example of Using `HttpException` for Custom Errors

```typescript
import {
  Controller,
  Get,
  Param,
  HttpException,
  HttpStatus,
} from "@nestjs/common";

@Controller("users")
export class UsersController {
  @Get(":id")
  findOne(@Param("id") id: string) {
    const user = this.findUserById(id);
    if (!user) {
      throw new HttpException(
        `User with ID ${id} not found`,
        HttpStatus.NOT_FOUND
      );
    }
    return user;
  }

  private findUserById(id: string) {
    // Simulate a database call that returns null if the user is not found
    return null; // Simulate user not found
  }
}
```

In this example:

- We use the `HttpException` class to throw a custom error when the user is not found.
- `HttpException` accepts two arguments: the error message and the HTTP status code.
- `HttpStatus.NOT_FOUND` is used to return a 404 status code in this case.

This way, if the user is not found, NestJS returns a JSON error response with the appropriate status code (404 in this case) and the custom error message.

#### Example of Using Built-In Exceptions

NestJS provides several built-in exceptions that extend `HttpException`. Here’s an example using `NotFoundException`, which is a subclass of `HttpException`.

```typescript
import { Controller, Get, Param, NotFoundException } from "@nestjs/common";

@Controller("users")
export class UsersController {
  @Get(":id")
  findOne(@Param("id") id: string) {
    const user = this.findUserById(id);
    if (!user) {
      throw new NotFoundException(`User with ID ${id} not found`);
    }
    return user;
  }

  private findUserById(id: string) {
    // Simulate user lookup logic
    return null; // Simulate user not found
  }
}
```

In this example, the `NotFoundException` is used to handle the "user not found" case. It automatically sets the HTTP status code to 404 and sends the provided message in the response.

#### Example of Creating Custom Exceptions

You can also create your own custom exceptions by extending the `HttpException` class.

```typescript
import { HttpException, HttpStatus } from "@nestjs/common";

export class CustomException extends HttpException {
  constructor() {
    super("This is a custom error message", HttpStatus.BAD_REQUEST);
  }
}
```

In your controller, you can throw this custom exception:

```typescript
@Controller("users")
export class UsersController {
  @Get(":id")
  findOne(@Param("id") id: string) {
    throw new CustomException(); // This will return a 400 Bad Request response
  }
}
```

Here, the `CustomException` class is used to return a 400 Bad Request response with a custom error message.

#### Handling Errors Summary

- **HttpException**: A generic class that you can use to throw custom errors with any HTTP status code and message.
- **Built-In Exceptions**: NestJS provides built-in exceptions like `NotFoundException`, `BadRequestException`, `UnauthorizedException`, etc., which are subclasses of `HttpException` and make handling common error cases easier.
- **Custom Exceptions**: You can extend `HttpException` to create your own custom exception classes.

These mechanisms make it straightforward to handle errors in a clear and consistent way, returning appropriate status codes and messages to the client.

#### Summary of Annotations for Controllers

- `@Controller('path')`: Defines the base route for the controller.
- `@Get()`, `@Post()`, `@Put()`, `@Delete()`, `@Patch()`: Handle specific HTTP methods.
- `@Param()`: Extracts **path parameters** from the route.
- `@Query()`: Extracts **query parameters** from the URL.
- `@Body()`: Extracts data from the **request body**.
- `@HttpCode()`: Sets the HTTP status code for a response.
- `@Res()`: Provides direct access to the response object for full control.

#### Conclusion

Controllers in NestJS serve as the gateway between incoming requests and the application's business logic. By using various decorators and handling path parameters, query parameters, and request bodies, controllers provide a flexible and easy way to define and manage routes. Moreover, they enable error handling and customization of responses, making them a powerful tool in building APIs with NestJS.
