# Security

To secure an API built using NestJS, several best practices should be implemented to mitigate vulnerabilities and ensure the protection of sensitive data. Below is a detailed explanation of how to secure a NestJS API, including authentication, authorization, input validation, and other key measures, followed by a practical example.

### 1. **Authentication**

Authentication is the process of verifying the identity of a user or system. There are several methods to authenticate users, such as **JWT (JSON Web Token)** or **OAuth 2.0**. In a NestJS application, authentication can be achieved by using the `@nestjs/passport` and `@nestjs/jwt` packages to implement JWT-based authentication.

#### Steps to Implement JWT Authentication:

1. **Install Required Packages**:

   ```bash
   npm install @nestjs/passport passport passport-jwt @nestjs/jwt
   ```

2. **Create Auth Module**:
   Generate an auth module to handle authentication logic:

   ```bash
   nest generate module auth
   ```

3. **Generate JWT Service**:
   The service will handle signing and verifying JWTs.

   ```ts
   // auth.service.ts
   @Injectable()
   export class AuthService {
     constructor(private jwtService: JwtService) {}

     async login(user: any) {
       const payload = { username: user.username, sub: user.userId };
       return {
         access_token: this.jwtService.sign(payload),
       };
     }

     async validateUser(username: string, pass: string): Promise<any> {
       // Validate user against database, etc.
     }
   }
   ```

4. **Protect Routes**:
   Protect your API routes using a JWT guard:

   ```ts
   // jwt-auth.guard.ts
   @Injectable()
   export class JwtAuthGuard extends AuthGuard("jwt") {}
   ```

   Apply the guard to your controller:

   ```ts
   @UseGuards(JwtAuthGuard)
   @Get('profile')
   getProfile(@Request() req) {
     return req.user;
   }
   ```

### 2. **Authorization**

Authorization ensures that authenticated users can only access the resources they are permitted to. This can be role-based or resource-based.

#### Role-based Access Control (RBAC):

You can create guards in NestJS that check for user roles before granting access to certain routes.

1. **Define Roles**:
   Create a decorator for roles:

   ```ts
   export const Roles = (...roles: Role[]) => SetMetadata("roles", roles);
   ```

2. **Create a Roles Guard**:
   Implement a roles guard to enforce role-based authorization.

   ```ts
   // roles.guard.ts
   @Injectable()
   export class RolesGuard implements CanActivate {
     canActivate(context: ExecutionContext): boolean {
       const request = context.switchToHttp().getRequest();
       const user = request.user;
       const hasRole = () => user.roles.includes(requiredRole);
       return user && user.roles && hasRole();
     }
   }
   ```

3. **Apply Roles to Routes**:
   Use the roles decorator in the controller:
   ```ts
   @UseGuards(JwtAuthGuard, RolesGuard)
   @Roles('admin')
   @Post('create')
   createResource(@Body() data) {
     // Admin-only access
   }
   ```

### 3. **Input Validation and Sanitization**

Input validation helps prevent injection attacks (e.g., SQL injection, XSS). NestJS has built-in support for validation using the `class-validator` and `class-transformer` packages.

1. **Install the Validator Packages**:

   ```bash
   npm install class-validator class-transformer
   ```

2. **Define DTOs (Data Transfer Objects)**:
   DTOs help validate the input sent to your API.

   ```ts
   // create-user.dto.ts
   export class CreateUserDto {
     @IsString()
     @MinLength(4)
     username: string;

     @IsString()
     @MinLength(8)
     password: string;
   }
   ```

3. **Apply Validation in Controller**:
   Use DTOs in your controller to validate requests.

   ```ts
   @Post('create-user')
   async createUser(@Body() createUserDto: CreateUserDto) {
     return this.userService.create(createUserDto);
   }
   ```

   Ensure the validation pipe is applied globally:

   ```ts
   app.useGlobalPipes(new ValidationPipe());
   ```

### 4. **Rate Limiting**

Rate limiting helps protect your API from brute force attacks by limiting the number of requests from a single client.

1. **Install Rate Limiter**:

   ```bash
   npm install @nestjs/throttler
   ```

2. **Configure the ThrottlerModule**:
   Apply rate limiting globally or at the route level.

   ```ts
   @Module({
     imports: [
       ThrottlerModule.forRoot({
         ttl: 60,
         limit: 10,
       }),
     ],
   })
   ```

3. **Use Rate Limiting in Controller**:
   ```ts
   @UseGuards(ThrottlerGuard)
   @Get('resource')
   getResource() {
     return this.resourceService.get();
   }
   ```

### 5. **CSRF Protection**

Cross-Site Request Forgery (CSRF) attacks can be mitigated by adding CSRF tokens to your requests. If you're building an application that works with a browser-based client, you can add CSRF tokens to protect form submissions.

For non-browser-based API interactions (like mobile apps or other services), you can omit this, but for web applications, CSRF is important.

### 6. **HTTPS (SSL/TLS)**

Always use HTTPS to encrypt the communication between the client and server. NestJS apps can be set up to work with HTTPS by configuring an SSL certificate and setting up a secure HTTP server.

```ts
const httpsOptions = {
  key: fs.readFileSync("path/to/private-key.pem"),
  cert: fs.readFileSync("path/to/public-certificate.pem"),
};

async function bootstrap() {
  const app = await NestFactory.create(AppModule, {
    httpsOptions,
  });
  await app.listen(3000);
}
```

### 7. **Secure Headers**

Using packages like `helmet` ensures that secure HTTP headers are set properly.

1. **Install Helmet**:

   ```bash
   npm install helmet
   ```

2. **Apply Helmet Middleware**:
   ```ts
   app.use(helmet());
   ```

### 8. **Logging and Monitoring**

Logging and monitoring can help detect malicious activities and security issues. Use logging tools such as **Winston** or **Elastic Stack** to capture API requests and errors.

```ts
const logger = new Logger("AppLogger");
app.useLogger(logger);
```

### Example Implementation

Here’s an example of how to combine JWT authentication, role-based authorization, and input validation in a NestJS API:

```ts
// user.controller.ts
@Controller("users")
export class UserController {
  constructor(private userService: UserService) {}

  @UseGuards(JwtAuthGuard, RolesGuard)
  @Roles("admin")
  @Post("create")
  async createUser(@Body() createUserDto: CreateUserDto) {
    return this.userService.create(createUserDto);
  }

  @UseGuards(JwtAuthGuard)
  @Get("profile")
  async getProfile(@Request() req) {
    return req.user;
  }
}
```

In this setup:

- **JWT Authentication** protects all routes.
- **Role-Based Authorization** ensures only admins can create new users.
- **Input Validation** guarantees user input meets defined rules before processing.

By following these steps, you can significantly enhance the security of your NestJS API.
