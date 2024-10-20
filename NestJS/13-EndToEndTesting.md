# End To End Testing

End-to-end (E2E) testing is a testing methodology that validates the complete functionality of an application, from start to finish, by simulating real user scenarios. In NestJS applications, E2E tests ensure that different components, such as controllers, services, and middleware, work together as expected. These tests typically involve starting the application and interacting with its public interfaces, such as APIs.

### Steps to Create End-to-End Tests in a NestJS Application using Jest

#### 1. **Set up Jest E2E Testing**

NestJS provides built-in support for Jest. To get started, make sure you have `@nestjs/testing`, `supertest`, and `jest` installed, which are used to simulate HTTP requests and perform assertions.

```bash
npm install --save-dev @nestjs/testing supertest jest
```

#### 2. **Create a Test File**

E2E test files are usually located in the `test` directory of your project and follow the naming convention `*.e2e-spec.ts`.

For example, for a `UserModule`, create the file `test/user.e2e-spec.ts`.

#### 3. **Set Up a Testing Module**

In the E2E tests, you need to create a testing instance of your application using `Test.createTestingModule()`. You can optionally start up a real HTTP server for handling HTTP requests.

Here's an example E2E test for a simple `UserModule` API:

```typescript
import { Test, TestingModule } from "@nestjs/testing";
import { INestApplication } from "@nestjs/common";
import * as request from "supertest";
import { UserModule } from "../src/user/user.module";

describe("UserModule (e2e)", () => {
  let app: INestApplication;

  beforeAll(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [UserModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();
  });

  afterAll(async () => {
    await app.close();
  });

  it("/users (GET)", () => {
    return request(app.getHttpServer()).get("/users").expect(200).expect({
      data: [],
    });
  });

  it("/users (POST)", () => {
    return request(app.getHttpServer())
      .post("/users")
      .send({ name: "John Doe", email: "john.doe@example.com" })
      .expect(201)
      .expect((res) => {
        expect(res.body.name).toBe("John Doe");
        expect(res.body.email).toBe("john.doe@example.com");
      });
  });
});
```

### Explanation of the Code:

1. **Set up the application**: `Test.createTestingModule()` creates a testing module with the `UserModule`. The `app.init()` initializes the NestJS application.
2. **Simulate requests with Supertest**: Supertest allows you to simulate HTTP requests to the application, as seen in the `request(app.getHttpServer())` call.
3. **Test example**: In the first test (`/users (GET)`), a `GET` request to the `/users` endpoint is made, expecting a status code of 200 and an empty data array. In the second test (`/users (POST)`), a `POST` request is made, simulating the creation of a new user and expecting a 201 status code with the correct name and email in the response.

#### 4. **Run the Tests**

To run the E2E tests, use the following command:

```bash
npm run test:e2e
```

### Example of Full E2E Workflow

- **Before each test**: Start up the application.
- **Simulate real user requests**: Perform `GET`, `POST`, `PUT`, `DELETE`, etc., requests using `supertest`.
- **Perform assertions**: Validate the response status, body, and other conditions.

#### 5. **Testing with a Database**

If your application interacts with a database, you can use an in-memory database (e.g., SQLite) or mock the database connections during the tests.

Example with an in-memory database:

```typescript
beforeAll(async () => {
  const moduleFixture: TestingModule = await Test.createTestingModule({
    imports: [
      UserModule,
      TypeOrmModule.forRoot({
        type: "sqlite",
        database: ":memory:",
        entities: [User],
        synchronize: true,
      }),
    ],
  }).compile();

  app = moduleFixture.createNestApplication();
  await app.init();
});
```

This way, you can test the application with a temporary, in-memory database, which is cleaned up after the tests.

### Best Practices

- **Reset the state after each test**: You can use lifecycle hooks like `beforeEach` or `afterEach` to reset the application or database.
- **Avoid external dependencies**: E2E tests should run in isolation, ideally without needing external services, to ensure test reliability.

This approach allows you to test the entire flow of your application, ensuring that all the components work together as expected.
