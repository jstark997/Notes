# Authentication In Detail

To enhance security in a NestJS API, authentication plays a crucial role. Two widely adopted authentication mechanisms are **JWT (JSON Web Token)** and **OAuth 2.0**. Both approaches have their specific use cases and levels of complexity.

Below, I'll explain step-by-step how to implement **JWT-based** and **OAuth-based** authentication in NestJS, including all the necessary code for both approaches.

---

## **JWT Authentication in NestJS**

JWT is a compact, self-contained way of securely transmitting information between parties as a JSON object. It’s most suitable for stateless authentication in APIs.

### **Step-by-Step Implementation of JWT Authentication**

#### 1. **Install Required Packages**

First, install the necessary packages:

```bash
npm install @nestjs/passport passport passport-jwt @nestjs/jwt bcryptjs
```

- `@nestjs/passport` provides a Passport.js integration for NestJS.
- `passport-jwt` implements JWT authentication strategy for Passport.js.
- `@nestjs/jwt` handles JWT creation and verification.
- `bcryptjs` is used for securely hashing and comparing passwords.

#### 2. **Create the Authentication Module**

Create a dedicated `auth` module to handle authentication logic:

```bash
nest generate module auth
```

#### 3. **Create the User Entity**

For this example, we assume that user data is stored in a relational database. You should create a `User` entity (or model) that holds user information, such as email, password, and roles.

```ts
// user.entity.ts
import { Entity, Column, PrimaryGeneratedColumn } from "typeorm";

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  username: string;

  @Column()
  password: string;

  @Column()
  roles: string[];
}
```

#### 4. **Create the AuthService**

The `AuthService` is responsible for verifying user credentials and generating the JWT.

```ts
// auth.service.ts
import { Injectable } from "@nestjs/common";
import { JwtService } from "@nestjs/jwt";
import { UsersService } from "../users/users.service";
import * as bcrypt from "bcryptjs";

@Injectable()
export class AuthService {
  constructor(
    private usersService: UsersService,
    private jwtService: JwtService
  ) {}

  async validateUser(username: string, pass: string): Promise<any> {
    const user = await this.usersService.findOne(username);
    if (user && (await bcrypt.compare(pass, user.password))) {
      const { password, ...result } = user;
      return result; // Return user details except for password
    }
    return null;
  }

  async login(user: any) {
    const payload = {
      username: user.username,
      sub: user.id,
      roles: user.roles,
    };
    return {
      access_token: this.jwtService.sign(payload),
    };
  }
}
```

#### 5. **Create the JWT Strategy**

The JWT strategy extracts the token from the request and validates it.

```ts
// jwt.strategy.ts
import { Injectable } from "@nestjs/common";
import { PassportStrategy } from "@nestjs/passport";
import { ExtractJwt, Strategy } from "passport-jwt";
import { jwtConstants } from "./constants";

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor() {
    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      secretOrKey: jwtConstants.secret,
    });
  }

  async validate(payload: any) {
    return {
      userId: payload.sub,
      username: payload.username,
      roles: payload.roles,
    };
  }
}
```

#### 6. **Set JWT Constants**

Define the JWT secret key that is used to sign the token. This should be stored securely (e.g., in environment variables).

```ts
// constants.ts
export const jwtConstants = {
  secret: "your-secret-key", // Store this in an environment variable in production
};
```

#### 7. **Register the JwtModule**

Register the `JwtModule` and provide it with the secret key and expiration time.

```ts
// auth.module.ts
import { Module } from "@nestjs/common";
import { JwtModule } from "@nestjs/jwt";
import { PassportModule } from "@nestjs/passport";
import { AuthService } from "./auth.service";
import { JwtStrategy } from "./jwt.strategy";
import { UsersModule } from "../users/users.module";
import { jwtConstants } from "./constants";

@Module({
  imports: [
    PassportModule,
    JwtModule.register({
      secret: jwtConstants.secret,
      signOptions: { expiresIn: "60s" },
    }),
    UsersModule,
  ],
  providers: [AuthService, JwtStrategy],
  exports: [AuthService],
})
export class AuthModule {}
```

#### 8. **Create the AuthController**

The `AuthController` provides the `/login` route to authenticate users and return a JWT.

```ts
// auth.controller.ts
import { Controller, Request, Post, UseGuards } from "@nestjs/common";
import { AuthService } from "./auth.service";

@Controller("auth")
export class AuthController {
  constructor(private authService: AuthService) {}

  @Post("login")
  async login(@Request() req) {
    return this.authService.login(req.body);
  }
}
```

#### 9. **Protecting Routes with Guards**

Now that JWT authentication is set up, you can protect specific routes by applying the `JwtAuthGuard`.

```ts
// jwt-auth.guard.ts
import { Injectable } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {}

// Example usage in a controller
@UseGuards(JwtAuthGuard)
@Get('profile')
getProfile(@Request() req) {
  return req.user;
}
```

---

## **OAuth 2.0 Authentication in NestJS**

OAuth 2.0 is a more complex authentication mechanism often used for third-party authentication (e.g., Google, Facebook, GitHub). In NestJS, Passport.js is commonly used for integrating OAuth strategies.

### **Step-by-Step Implementation of OAuth 2.0 Authentication**

#### 1. **Install Required Packages**

For OAuth 2.0, you need Passport.js strategies for the specific OAuth provider you're using. For example, to implement **Google OAuth**, install the following:

```bash
npm install @nestjs/passport passport passport-google-oauth20
```

- `passport-google-oauth20`: Google OAuth 2.0 strategy for Passport.js.

#### 2. **Configure Google OAuth Credentials**

You must first create OAuth credentials from [Google API Console](https://console.developers.google.com/), which will provide you with a `clientID` and `clientSecret`.

#### 3. **Create the Google OAuth Strategy**

Set up a strategy for handling Google OAuth 2.0.

```ts
// google.strategy.ts
import { Injectable } from "@nestjs/common";
import { PassportStrategy } from "@nestjs/passport";
import { Strategy, VerifyCallback } from "passport-google-oauth20";

@Injectable()
export class GoogleStrategy extends PassportStrategy(Strategy, "google") {
  constructor() {
    super({
      clientID: "your-google-client-id",
      clientSecret: "your-google-client-secret",
      callbackURL: "http://localhost:3000/auth/google/callback",
      scope: ["email", "profile"],
    });
  }

  async validate(
    accessToken: string,
    refreshToken: string,
    profile: any,
    done: VerifyCallback
  ): Promise<any> {
    const { name, emails, photos } = profile;
    const user = {
      email: emails[0].value,
      firstName: name.givenName,
      lastName: name.familyName,
      picture: photos[0].value,
      accessToken,
    };
    done(null, user);
  }
}
```

#### 4. **Create the OAuth Controller**

Define the controller that handles the OAuth routes.

```ts
// auth.controller.ts
import { Controller, Get, Req, UseGuards } from "@nestjs/common";
import { AuthGuard } from "@nestjs/passport";

@Controller("auth")
export class AuthController {
  @Get("google")
  @UseGuards(AuthGuard("google"))
  async googleAuth(@Req() req) {
    // Initiates Google OAuth
  }

  @Get("google/callback")
  @UseGuards(AuthGuard("google"))
  async googleAuthRedirect(@Req() req) {
    // Handles the OAuth callback from Google
    return {
      message: "User information from Google",
      user: req.user,
    };
  }
}
```

#### 5. **Add Google Strategy to AuthModule**

Ensure the Google strategy is registered in your `AuthModule`.

```ts
// auth.module.ts
import { Module } from "@nestjs/common";
import { PassportModule } from "@nestjs/passport";
import { AuthController } from "./auth.controller";
import { GoogleStrategy } from "./google.strategy";

@Module({
  imports: [PassportModule.register({ defaultStrategy: "google" })],
  controllers: [AuthController],
  providers: [GoogleStrategy],
})
export class AuthModule {}
```

#### 6. **Protect Routes**

You can now protect routes by using the `AuthGuard` for Google OAuth. The login flow will redirect the user to Google’s login page, and after successful authentication, the user will be redirected back to the application with their profile information.

---

## **API Keys in NestJS**

API keys are another form of authentication that is commonly used to control access to an API. While less secure compared to OAuth or JWT, API keys are simple to implement and can be effective for securing services like internal APIs, microservices, or third-party integrations where user authentication is not required. However, API keys should be combined with other security measures (e.g., rate limiting, IP whitelisting) to enhance security.

Here’s how API keys fit in with other authentication techniques:

- **API keys** are **lightweight** and simple to implement but offer less granular control and security compared to JWT or OAuth 2.0.
- They are **stateless**, like JWT, meaning that the server doesn’t need to store the session state, which makes them a good fit for distributed systems.
- They are **not tied to a specific user** identity. API keys authenticate the caller, but they don’t typically provide detailed identity or role information like JWT or OAuth does.

API keys are useful in scenarios like:

- Accessing internal microservices.
- Securing third-party integrations where user authentication is not necessary.
- Systems where the client is trusted (such as a server-to-server API).

### **How API Key Authentication Works**

An API key is typically a long, unique string that the client includes in the header or query string of the API request. The server checks this key to authorize the request.

---

### **Step-by-Step Implementation of API Key Authentication in NestJS**

### 1. **Generate and Store API Keys**

First, you need a mechanism to issue API keys and associate them with specific clients or services. For simplicity, in this example, we’ll assume that API keys are stored in a database or environment variable.

You could generate the API keys manually or use a service to generate random keys.

Example of a simple key:

```ts
export const apiKeys = {
  service1: "api-key-123456",
  service2: "api-key-7891011",
};
```

### 2. **Create an API Key Guard**

You can implement an `APIKeyGuard` to validate the API key included in the request. This guard will intercept incoming requests, extract the key, and check if it matches any valid keys.

```ts
// api-key.guard.ts
import {
  Injectable,
  CanActivate,
  ExecutionContext,
  UnauthorizedException,
} from "@nestjs/common";
import { Reflector } from "@nestjs/core";
import { Request } from "express";
import { apiKeys } from "./api-keys.constants";

@Injectable()
export class ApiKeyGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const request = context.switchToHttp().getRequest<Request>();
    const apiKey = request.headers["x-api-key"];

    if (!apiKey) {
      throw new UnauthorizedException("API key is missing");
    }

    const isValidKey = Object.values(apiKeys).includes(apiKey);
    if (!isValidKey) {
      throw new UnauthorizedException("Invalid API key");
    }

    return true;
  }
}
```

In this guard:

- The API key is expected in the request header under the name `x-api-key`.
- It checks whether the API key is included in the predefined list of valid keys (in this case, defined in `apiKeys`).

### 3. **Apply API Key Guard to Protected Routes**

Once the guard is created, you can apply it to the routes that need protection. You can either apply it globally (for all routes) or selectively to certain controllers or methods.

#### Apply Globally:

You can set the `ApiKeyGuard` globally in the main application file (`main.ts`) so that it protects all routes by default:

```ts
// main.ts
import { NestFactory } from "@nestjs/core";
import { AppModule } from "./app.module";
import { ApiKeyGuard } from "./auth/api-key.guard";

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalGuards(new ApiKeyGuard());
  await app.listen(3000);
}
bootstrap();
```

#### Apply to Specific Routes:

Alternatively, you can apply the `ApiKeyGuard` to specific routes within a controller.

```ts
// app.controller.ts
import { Controller, Get, UseGuards } from "@nestjs/common";
import { ApiKeyGuard } from "./auth/api-key.guard";

@Controller("data")
export class DataController {
  @UseGuards(ApiKeyGuard)
  @Get()
  getData() {
    return { message: "Protected data" };
  }
}
```

### 4. **Handling API Key Validation**

The `ApiKeyGuard` uses a constant object (`apiKeys`) to validate API keys in this example. In a real-world scenario, you would probably store API keys in a more secure and scalable way, such as:

- **Environment Variables**: For simple use cases where the number of clients is small.
- **Database**: Storing API keys along with other metadata like the client name, roles, and expiration date.

Here’s an example using a database (e.g., with TypeORM):

#### Define an API Key Entity:

```ts
// api-key.entity.ts
import { Entity, Column, PrimaryGeneratedColumn } from "typeorm";

@Entity()
export class ApiKey {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  key: string;

  @Column()
  serviceName: string;
}
```

#### Create the API Key Service:

```ts
// api-key.service.ts
import { Injectable } from "@nestjs/common";
import { InjectRepository } from "@nestjs/typeorm";
import { Repository } from "typeorm";
import { ApiKey } from "./api-key.entity";

@Injectable()
export class ApiKeyService {
  constructor(
    @InjectRepository(ApiKey)
    private apiKeyRepository: Repository<ApiKey>
  ) {}

  async validateApiKey(apiKey: string): Promise<boolean> {
    const key = await this.apiKeyRepository.findOne({ where: { key: apiKey } });
    return !!key;
  }
}
```

#### Modify the Guard to Use the Service:

```ts
// api-key.guard.ts
import {
  Injectable,
  CanActivate,
  ExecutionContext,
  UnauthorizedException,
} from "@nestjs/common";
import { Request } from "express";
import { ApiKeyService } from "./api-key.service";

@Injectable()
export class ApiKeyGuard implements CanActivate {
  constructor(private apiKeyService: ApiKeyService) {}

  async canActivate(context: ExecutionContext): Promise<boolean> {
    const request = context.switchToHttp().getRequest<Request>();
    const apiKey = request.headers["x-api-key"];

    if (!apiKey) {
      throw new UnauthorizedException("API key is missing");
    }

    const isValid = await this.apiKeyService.validateApiKey(apiKey);
    if (!isValid) {
      throw new UnauthorizedException("Invalid API key");
    }

    return true;
  }
}
```

### 5. **Test the API Key Authentication**

You can now test the API key authentication by making requests to the protected route with and without the API key.

#### Request Without API Key:

```bash
curl -X GET http://localhost:3000/data
```

Response:

```json
{
  "statusCode": 401,
  "message": "API key is missing"
}
```

#### Request With Invalid API Key:

```bash
curl -X GET http://localhost:3000/data -H "x-api-key: invalid-key"
```

Response:

```json
{
  "statusCode": 401,
  "message": "Invalid API key"
}
```

#### Request With Valid API Key:

```bash
curl -X GET http://localhost:3000/data -H "x-api-key: api-key-123456"
```

Response:

```json
{
  "message": "Protected data"
}
```

---

### **Best Practices for Using API Keys**

- **Use Over HTTPS**: Always use HTTPS to prevent exposure of API keys in transit.
- **Rotate API Keys**: Periodically rotate API keys and support revoking compromised keys.
- **Rate Limiting**: Combine API key authentication with rate limiting to protect your API from abuse.
- **IP Whitelisting**: For additional security, restrict access to certain IP addresses associated with the API key.
- **Logging**: Track usage of API keys to monitor and audit client behavior.

---

### **Comparison of API Keys, JWT and OAuth 2.0**

| Feature                   | **API Keys**                                | **JWT**                                 | **OAuth 2.0**                               |
| ------------------------- | ------------------------------------------- | --------------------------------------- | ------------------------------------------- |
| **Use Case**              | Simple API authentication                   | User-based authentication               | Third-party authentication                  |
| **Security**              | Relatively weak without additional measures | Strong due to signed tokens             | Strong, relies on third-party providers     |
| **Granularity**           | Generally tied to a service, not users      | User and role-based authentication      | User-based with third-party providers       |
| **Complexity**            | Low                                         | Moderate                                | High                                        |
| **Statefulness**          | Stateless                                   | Stateless                               | State maintained by provider                |
| **Token Expiry Handling** | Manual rotation or revocation               | Token expiration and refresh supported  | Provider handles token expiration           |
| **Use in Public APIs**    | Common for simple use cases                 | Preferred for secured user interactions | Ideal for social login and delegated access |

---

API keys provide a lightweight and simple way to authenticate access to your API, especially for internal services and third-party integrations. However, due to their limited security features, API keys should be used in conjunction with other security measures for sensitive applications.
