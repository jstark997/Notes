# Unit Testing

Unit testing in the context of NestJS involves testing individual components (such as services or controllers) in isolation from the rest of the system. The goal is to ensure that each part behaves as expected, independent of its external dependencies. In order to test these components in isolation, mocks are often used to simulate the behavior of dependencies, and Jest, the testing framework, provides tools to facilitate this.

### What is `jest.fn()`?

In Jest, `jest.fn()` is a powerful utility that allows you to create mock functions. These mock functions can simulate real functions, track how they are called, and even provide specific return values or behaviors when they are invoked. This is particularly useful for unit testing, where you want to isolate the functionality of a component by mocking its dependencies.

- **Basic Usage**: A mock function created using `jest.fn()` can replace real dependencies during testing.
- **Tracking**: Jest mock functions track calls, arguments, return values, and more.
- **Customization**: You can specify the return value or behavior of a mock function (e.g., returning a promise or throwing an error) using methods like `mockResolvedValue` and `mockRejectedValue`.

### Setting Up Unit Tests in NestJS Using Jest

#### 1. **Creating Unit Tests for a Service**

Let's assume we have a `UserService` that fetches user data from a repository or an external API. A basic test would focus on one method in this service.

```typescript
// user.service.ts
import { Injectable } from "@nestjs/common";
import { UserRepository } from "./user.repository";

@Injectable()
export class UserService {
  constructor(private readonly userRepository: UserRepository) {}

  async getUserById(id: string) {
    const user = await this.userRepository.findById(id);
    if (!user) {
      throw new Error("User not found");
    }
    return user;
  }
}
```

Now, let's write a unit test for the `getUserById` method in `UserService`. We'll mock the `UserRepository` dependency using `jest.fn()`.

```typescript
// user.service.spec.ts
import { Test, TestingModule } from "@nestjs/testing";
import { UserService } from "./user.service";
import { UserRepository } from "./user.repository";

describe("UserService", () => {
  let service: UserService;

  // Mocking the UserRepository using jest.fn()
  let mockUserRepository = {
    findById: jest.fn(), // This creates a mock function for the findById method
  };

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        UserService,
        { provide: UserRepository, useValue: mockUserRepository }, // Injecting the mock repository
      ],
    }).compile();

    service = module.get<UserService>(UserService);
  });

  it("should return user when user is found", async () => {
    const user = { id: "1", name: "John Doe" };

    // Mocking the return value of findById using mockResolvedValue (used for async functions)
    mockUserRepository.findById.mockResolvedValue(user);

    expect(await service.getUserById("1")).toEqual(user);
    expect(mockUserRepository.findById).toHaveBeenCalledWith("1"); // Verifying the mock was called correctly
  });
});
```

### Explanation of the Mock

In the test above, we used `jest.fn()` to create a mock function for `findById` in the `UserRepository`. This allows us to define its behavior without calling the real implementation:

- `mockResolvedValue(user)` tells the mock to return a resolved promise containing `user` when called.
- The mock function tracks how it was called, so we can verify that `findById` was called with the correct arguments.

#### 2. **Creating Mock Services**

The `mockUserRepository` object contains a mock implementation of the `findById` method. We pass this mock into the NestJS testing module via `useValue`:

```typescript
let mockUserRepository = {
  findById: jest.fn(), // Mocking the findById method
};
```

This approach allows you to control the behavior of the repository during testing. The real `UserRepository` isn’t called, ensuring the test focuses solely on the logic in `UserService`.

#### 3. **Testing for Exceptions**

To test that the `UserService` throws an error when a user is not found, we can add a test that expects an exception. This shows how we can use `jest.fn()` to simulate different scenarios.

```typescript
it("should throw an error when user is not found", async () => {
  // Simulate a scenario where the repository returns null (no user found)
  mockUserRepository.findById.mockResolvedValue(null);

  // Using rejects.toThrow to check that an exception is thrown
  await expect(service.getUserById("1")).rejects.toThrow("User not found");
});
```

Here’s what happens in this test:

- `mockResolvedValue(null)` sets the return value of `findById` to `null`, simulating the case where no user is found.
- `await expect(service.getUserById('1'))` returns a promise, and `rejects.toThrow` checks if the promise is rejected with the expected error.

### Summary of Steps:

1. **Unit Test Setup**: Use `@nestjs/testing` to create a testing module and inject the service under test.
2. **Mock Dependencies with `jest.fn()`**: Use Jest’s `jest.fn()` to create mock functions for dependencies like repositories or services.
3. **Write Test Cases**:
   - Verify normal functionality by controlling the return values of the mock functions.
   - Use `rejects.toThrow` to verify that exceptions are handled correctly.

By using `jest.fn()` to mock dependencies, you can effectively isolate and test your NestJS services in unit tests without relying on real implementations of external services or databases.
