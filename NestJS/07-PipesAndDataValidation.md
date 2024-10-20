# Pipes And Data Validation

In NestJS, **pipes** are a powerful feature used to transform and validate data coming into a controller before it reaches the route handler. Pipes operate by either transforming the data or validating it and throwing errors if the validation fails. NestJS provides a built-in pipe called `ValidationPipe` that simplifies data validation, especially for incoming HTTP requests.

#### What is a Pipe?

A **pipe** in NestJS is a class annotated with the `@Injectable()` decorator that implements the `PipeTransform` interface. Pipes have two primary functions:

1. **Transformation**: Transform incoming data into the format the application expects.
2. **Validation**: Validate the incoming data and throw an exception if the data does not meet certain criteria.

Pipes are executed after the controller's method parameters are resolved but before they reach the route handler. You can apply pipes at different levels:

- **Globally**: Apply the pipe to all routes in the application.
- **At the controller or route level**: Apply the pipe to specific controllers or routes.

#### What is `ValidationPipe`?

`ValidationPipe` is a built-in pipe that provides an easy way to validate incoming request data using classes and decorators from the `class-validator` and `class-transformer` libraries. When the `ValidationPipe` is applied, it automatically validates the incoming request body against predefined rules and constraints defined in Data Transfer Objects (DTOs).

If the validation fails, the `ValidationPipe` throws an exception and returns a detailed error response to the client, indicating why the validation failed.

#### Installing Required Libraries

To use `ValidationPipe`, you need to install two additional libraries: `class-validator` and `class-transformer`.

```bash
npm install class-validator class-transformer
```

- `class-validator`: Provides decorators for defining validation rules.
- `class-transformer`: Automatically transforms plain JavaScript objects into instances of classes.

#### Setting Up `ValidationPipe`

To enable validation across the entire application, you can configure `ValidationPipe` in the `main.ts` file as a **global pipe**. This ensures that all incoming requests are validated.

```typescript
// main.ts
import { NestFactory } from "@nestjs/core";
import { AppModule } from "./app.module";
import { ValidationPipe } from "@nestjs/common";

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Enable global validation using ValidationPipe
  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true, // Strip out properties that are not decorated in the DTO, default is false
      forbidNonWhitelisted: true, // Throw an error if any non-whitelisted properties are present, default is false
      transform: true, // Automatically transform request payloads into DTO objects, default is false
    })
  );

  await app.listen(3000);
}
bootstrap();
```

In the above example, the `ValidationPipe` is set up globally:

- `whitelist: true`: Validator will strip validated (returned) object of any properties that do not use any validation decorators.
- `forbidNonWhitelisted: true`: Instead of stripping non-whitelisted properties validator will throw an exception.
- `transform: true`: Automatically converts plain JSON objects to instances of the DTO classes (using `class-transformer`).

**Note** - None of the above properties are necessary for basic data validation.

#### Using `ValidationPipe` in Controllers

To use the `ValidationPipe` in a specific route or controller, you can apply it using the `@Body()` decorator.

Let’s say you’re building a user registration feature. You want to validate the incoming request body to ensure that it contains a valid name, email, and password.

1. **Define a Data Transfer Object (DTO) for validation**:

```typescript
// create-user.dto.ts
import { IsString, IsEmail, IsNotEmpty, MinLength } from "class-validator";

export class CreateUserDto {
  @IsString()
  @IsNotEmpty()
  name: string;

  @IsEmail()
  email: string;

  @IsString()
  @MinLength(6)
  password: string;
}
```

In this DTO:

- `@IsString()` ensures that the `name` and `password` fields are strings.
- `@IsEmail()` ensures that the `email` field contains a valid email address.
- `@IsNotEmpty()` ensures that the `name` field is not empty.
- `@MinLength(6)` ensures that the `password` field has a minimum length of 6 characters.

2. **Use `ValidationPipe` in the controller**:

```typescript
import { Controller, Post, Body } from "@nestjs/common";
import { CreateUserDto } from "./create-user.dto";

@Controller("users")
export class UsersController {
  @Post("register")
  registerUser(@Body(new ValidationPipe()) createUserDto: CreateUserDto) {
    return `User ${createUserDto.name} registered successfully`;
  }
}
```

In this example:

- The `@Body(new ValidationPipe())` applies the `ValidationPipe` to the `registerUser()` method, ensuring that the incoming request body matches the validation rules defined in `CreateUserDto`.

#### Example Request and Validation

Here’s how it works in practice:

- **Valid request**:

```json
POST /users/register
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "secret123"
}
```

This request passes validation, and the server responds with:

```json
{
  "message": "User John Doe registered successfully"
}
```

- **Invalid request** (missing required `name` field and invalid email):

```json
POST /users/register
{
  "email": "invalid-email",
  "password": "secret123"
}
```

This request fails validation, and the server responds with an error:

```json
{
  "statusCode": 400,
  "message": ["name should not be empty", "email must be an email"],
  "error": "Bad Request"
}
```

#### Using ValidationPipe Globally vs. Locally

- **Globally**: Setting up `ValidationPipe` globally (in `main.ts`) applies validation across all incoming requests.
- **Locally**: You can also apply `ValidationPipe` at the controller or route level using `@Body(new ValidationPipe())` to handle validation for specific routes only.

#### Custom Error Messages

You can also customize the error messages by passing additional options to the decorators:

```typescript
import { IsString, IsEmail, IsNotEmpty, MinLength } from "class-validator";

export class CreateUserDto {
  @IsString({ message: "Name must be a string" })
  @IsNotEmpty({ message: "Name cannot be empty" })
  name: string;

  @IsEmail({}, { message: "Invalid email address" })
  email: string;

  @IsString()
  @MinLength(6, { message: "Password must be at least 6 characters long" })
  password: string;
}
```

With these custom messages, the error responses will be more descriptive for the client.

#### Summary of `ValidationPipe` Features

- **Automatic validation**: Validates incoming request data based on DTOs using `class-validator`.
- **Transformation**: Converts plain JSON objects into instances of your DTO classes using `class-transformer`.
- **Whitelist**: Removes any properties not decorated in the DTO (when enabled).
- **Forbid non-whitelisted properties**: Throws an error if non-whitelisted properties are present (when enabled).
- **Custom error messages**: Customizes error messages for failed validations.

By using `ValidationPipe`, you can ensure that your application enforces data integrity and properly handles incoming request data in a clear, concise, and modular way.
