# Environment Variables

Environment variables in a NestJS application are used to store configuration values that might change between environments, such as development, staging, and production. These values can include database connection strings, API keys, port numbers, and other sensitive or environment-specific information. The key benefit of using environment variables is that they allow the same application code to be reused across different environments while only needing to change the environment-specific variables.

### Using Environment Variables in a NestJS Application

To work with environment variables in NestJS, you can use the built-in `ConfigModule`, which provides a clean and scalable way to manage environment variables. Here’s how to set them up:

### Step 1: Install the `@nestjs/config` package

First, you need to install the `@nestjs/config` package, which allows you to load and use environment variables in a NestJS app.

```bash
npm install @nestjs/config
```

### Step 2: Create a `.env` file

In the root directory of your NestJS project, create a `.env` file. This file will store your environment variables.

Here’s an example `.env` file:

```env
PORT=3000
DATABASE_URL=mysql://user:password@localhost:3306/mydb
JWT_SECRET=supersecretkey
```

### Step 3: Import and Configure `ConfigModule`

In your `AppModule`, import and configure the `ConfigModule` to load the `.env` file:

```typescript
import { Module } from "@nestjs/common";
import { ConfigModule } from "@nestjs/config";

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true, // Makes the config available globally in the application
    }),
  ],
  // other imports and providers
})
export class AppModule {}
```

The `isGlobal: true` option ensures that the `ConfigModule` is available globally, so you don’t have to import it into every module.

### Step 4: Access Environment Variables in Your Code

You can now access your environment variables using the `ConfigService` in your services, controllers, or anywhere in the application. For example, to access the `PORT` and `DATABASE_URL`:

```typescript
import { Injectable } from "@nestjs/common";
import { ConfigService } from "@nestjs/config";

@Injectable()
export class AppService {
  constructor(private configService: ConfigService) {}

  getPort(): string {
    return this.configService.get<string>("PORT");
  }

  getDatabaseUrl(): string {
    return this.configService.get<string>("DATABASE_URL");
  }
}
```

In the above example, `this.configService.get<string>('PORT')` retrieves the value of the `PORT` environment variable, and `this.configService.get<string>('DATABASE_URL')` retrieves the `DATABASE_URL`.

### Example of Usage in Controller

```typescript
import { Controller, Get } from "@nestjs/common";
import { AppService } from "./app.service";

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get("port")
  getPort(): string {
    return this.appService.getPort();
  }
}
```

### Handling Default Values

You can also provide default values in case the environment variable is missing. For example:

```typescript
const port = this.configService.get<number>("PORT", 3000); // Default to 3000 if PORT is not set
```

### Step 5: Using Environment Variables in Other Parts of the App

In addition to service classes, you can also use environment variables in other parts of your NestJS app, such as guards, interceptors, and middlewares, by injecting `ConfigService` in those classes.

### Best Practices

- **Do not commit `.env` files**: The `.env` file often contains sensitive information like API keys and passwords. Make sure to add it to `.gitignore` to avoid committing it to version control.
- **Use `ConfigModule.forRoot({ isGlobal: true })` carefully**: While setting the configuration module as global simplifies importing, it’s best to be cautious when working with large applications to avoid tight coupling between different modules.

- **Use type-safe configuration**: You can use the `ConfigModule.forRoot({ validationSchema: Joi.object({...}) })` to validate and type-check environment variables.

This setup will make your NestJS application flexible and secure, allowing you to configure it for different environments without changing the codebase.

### Using `forAsyncRoot` in NestJS

The `forAsyncRoot` method on the `ConfigModule` is used to configure environment variables asynchronously, allowing you to load dynamic configurations based on the runtime environment, such as different database configurations for development, testing, or production. This is particularly useful when certain values (e.g., database names) need to change dynamically based on the environment.

### Example: Using Different SQLite Databases Based on the Environment

Suppose you are using SQLite as your database, and you want to use different database files based on whether you are running in a development, test, or production environment. You can achieve this with `forAsyncRoot` by loading environment-specific configurations dynamically.

### Step 1: Install `@nestjs/config` Package

```bash
npm install @nestjs/config
```

### Step 2: Create Environment Files

You can create different `.env` files for each environment:

- **.env.development**

  ```env
  DATABASE_NAME=dev_database.sqlite
  ```

- **.env.test**

  ```env
  DATABASE_NAME=test_database.sqlite
  ```

### Step 3: Configure `ConfigModule` with `forAsyncRoot`

In your `AppModule`, you can configure `ConfigModule` to dynamically load the correct database file based on the environment using the `forAsyncRoot` method:

```typescript
import { Module } from "@nestjs/common";
import { ConfigModule, ConfigService } from "@nestjs/config";
import { TypeOrmModule } from "@nestjs/typeorm";

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      envFilePath: `.env.${process.env.NODE_ENV || "development"}`, // Load the right .env file
    }),
    TypeOrmModule.forRootAsync({
      imports: [ConfigModule],
      inject: [ConfigService],
      useFactory: (configService: ConfigService) => ({
        type: "sqlite",
        database: configService.get<string>("DATABASE_NAME"),
        entities: [__dirname + "/../**/*.entity{.ts,.js}"],
        synchronize: true, // Only for dev, turn off in production
      }),
    }),
  ],
})
export class AppModule {}
```

### Step 4: Setting the `NODE_ENV` Variable

To ensure the correct `.env` file is loaded, set the `NODE_ENV` environment variable when running the application. For example:

- For **development**:

  ```bash
  NODE_ENV=development npm run start
  ```

- For **testing**:

  ```bash
  NODE_ENV=test npm run test
  ```

### Explanation

- `ConfigModule.forRoot` is used to load the correct `.env` file based on the `NODE_ENV` environment variable. If `NODE_ENV` is not set, it defaults to `development`.
- `TypeOrmModule.forRootAsync` uses the `ConfigService` to asynchronously configure the database connection. The `useFactory` function is used to retrieve the `DATABASE_NAME` from the environment variables and pass it to the database connection configuration.

### Benefits of `forAsyncRoot`

- **Asynchronous Loading**: The `forAsyncRoot` method allows for complex configurations that might require asynchronous processing, such as fetching configurations from an external source (like a database or an API).
- **Dynamic Configuration**: You can easily switch between environments by providing environment-specific configurations, making your application flexible and adaptable to different scenarios.

### Example Use in a Service

Let’s assume you want to log the name of the database being used in your service for debugging purposes:

```typescript
import { Injectable } from "@nestjs/common";
import { ConfigService } from "@nestjs/config";

@Injectable()
export class AppService {
  constructor(private configService: ConfigService) {}

  getDatabaseName(): string {
    return this.configService.get<string>("DATABASE_NAME");
  }
}
```

Then, you can call this service in a controller or any part of the application:

```typescript
import { Controller, Get } from "@nestjs/common";
import { AppService } from "./app.service";

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get("db-name")
  getDatabaseName(): string {
    return this.appService.getDatabaseName();
  }
}
```

### Conclusion

The `forAsyncRoot` method in NestJS provides a powerful way to manage environment variables asynchronously, allowing for flexible and dynamic configurations based on runtime conditions. By leveraging this capability, you can ensure that different environments (e.g., development, testing) are properly handled with distinct configurations such as different SQLite databases.
