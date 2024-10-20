# Jest fn Function

`jest.fn()` is a utility in Jest that allows you to create mock functions, which can simulate the behavior of real functions in your code. Mock functions are useful in unit testing, where the goal is to isolate the functionality of the unit being tested by controlling its dependencies.

Here’s a more detailed explanation of `jest.fn()` and examples of its various uses:

### 1. **Basic Usage of `jest.fn()`**

When you create a mock function using `jest.fn()`, it doesn’t do anything by default, but it can still track how many times it’s been called, what arguments it received, and what the return values were.

Example:

```typescript
const mockFunction = jest.fn();

mockFunction("Hello", "World");

// Check if the mock function was called
expect(mockFunction).toHaveBeenCalled();

// Check if it was called with specific arguments
expect(mockFunction).toHaveBeenCalledWith("Hello", "World");

// Check how many times it was called
expect(mockFunction).toHaveBeenCalledTimes(1);
```

In this example:

- `jest.fn()` creates an empty mock function.
- The `mockFunction` is then called with two arguments.
- We use assertions to verify that it was called, called with the correct arguments, and called exactly once.

### 2. **Providing Return Values**

You can configure a mock function to return specific values when it is called. This can be done using `mockReturnValue`, `mockReturnValueOnce`, or by passing an implementation to `jest.fn()`.

#### `mockReturnValue()`

Sets a return value that will be returned every time the mock function is called.

Example:

```typescript
const mockFunction = jest.fn().mockReturnValue(42);

expect(mockFunction()).toBe(42); // Always returns 42
expect(mockFunction()).toBe(42); // Always returns 42
```

#### `mockReturnValueOnce()`

This method allows you to specify a return value that will be used only the next time the function is called. You can chain this to set up different return values for consecutive calls.

Example:

```typescript
const mockFunction = jest
  .fn()
  .mockReturnValueOnce("first call")
  .mockReturnValueOnce("second call")
  .mockReturnValue("default");

expect(mockFunction()).toBe("first call"); // First call
expect(mockFunction()).toBe("second call"); // Second call
expect(mockFunction()).toBe("default"); // Subsequent calls return the default value
expect(mockFunction()).toBe("default"); // Still returns the default value
```

### 3. **Simulating Asynchronous Functions**

You can mock functions that return promises using `mockResolvedValue()` and `mockRejectedValue()`, which are especially useful when dealing with asynchronous functions.

#### `mockResolvedValue()`

This is used for mocking functions that return a resolved promise.

Example:

```typescript
const mockAsyncFunction = jest.fn().mockResolvedValue("resolved value");

await expect(mockAsyncFunction()).resolves.toBe("resolved value");
```

#### `mockRejectedValue()`

This is used for mocking functions that return a rejected promise.

Example:

```typescript
const mockAsyncFunction = jest
  .fn()
  .mockRejectedValue(new Error("error occurred"));

await expect(mockAsyncFunction()).rejects.toThrow("error occurred");
```

### 4. **Mocking Implementations**

You can define custom behavior for a mock function by passing a function to `jest.fn()`. This is useful when you want the mock function to perform specific logic or calculations based on the input.

Example:

```typescript
const mockFunction = jest.fn((a, b) => a + b);

expect(mockFunction(1, 2)).toBe(3); // 1 + 2 = 3
expect(mockFunction(3, 4)).toBe(7); // 3 + 4 = 7
expect(mockFunction).toHaveBeenCalledWith(1, 2);
```

In this case, the mock function sums the arguments provided to it.

### 5. **Tracking Calls**

Mock functions automatically track how they are called, including:

- **Call count**
- **Arguments passed in each call**
- **Return values for each call**
- **Instances created (when using as a constructor)**

Example:

```typescript
const mockFunction = jest.fn();

mockFunction("arg1");
mockFunction("arg2");

expect(mockFunction).toHaveBeenCalledTimes(2); // Called twice
expect(mockFunction).toHaveBeenNthCalledWith(1, "arg1"); // First call with 'arg1'
expect(mockFunction).toHaveBeenNthCalledWith(2, "arg2"); // Second call with 'arg2'
```

### 6. **Mocking Constructors**

If you want to mock a class, `jest.fn()` can simulate the constructor and its methods. This allows you to track how many times the constructor was called, with which arguments, and how its methods behave.

Example:

```typescript
class SomeClass {
  someMethod() {
    return "real method";
  }
}

const MockedClass = jest.fn().mockImplementation(() => ({
  someMethod: jest.fn().mockReturnValue("mocked method"),
}));

const instance = new MockedClass();
expect(instance.someMethod()).toBe("mocked method");
expect(MockedClass).toHaveBeenCalled(); // Verifies the constructor was called
```

### 7. **Clearing and Resetting Mocks**

Sometimes, you may want to clear or reset the state of a mock function between tests.

- **`mockClear()`**: Clears the call history of the mock.
- **`mockReset()`**: Clears the call history and also removes any return values or implementations.
- **`mockRestore()`**: Restores the original (non-mocked) implementation, if the mock was a spy on an existing function.

Example:

```typescript
const mockFunction = jest.fn().mockReturnValue(42);

mockFunction("arg1");
expect(mockFunction).toHaveBeenCalledTimes(1);

mockFunction.mockClear(); // Clears call history, but keeps return value

expect(mockFunction()).toBe(42);
expect(mockFunction).toHaveBeenCalledTimes(1);

mockFunction.mockReset(); // Clears call history and removes return value

mockFunction();
expect(mockFunction).toHaveBeenCalledTimes(1); // But now returns undefined by default
```

### 8. **Mocking Modules**

Sometimes you need to mock an entire module, and `jest.fn()` plays a crucial role here. Jest’s `jest.mock()` function allows you to mock the entire module and replace specific functions with `jest.fn()` mocks.

Example:

```typescript
// math.ts
export const add = (a: number, b: number) => a + b;
export const subtract = (a: number, b: number) => a - b;

// test file
import * as math from "./math";

jest.mock("./math", () => ({
  add: jest.fn().mockReturnValue(10),
  subtract: jest.fn().mockReturnValue(5),
}));

expect(math.add(2, 3)).toBe(10); // Mocked return value
expect(math.subtract(5, 2)).toBe(5); // Mocked return value
```

### Summary of Key Features of `jest.fn()`:

1. **Create mock functions**: `jest.fn()` creates a mock function that can be customized.
2. **Track call history**: Mock functions track how they are called (arguments, call count, etc.).
3. **Control return values**: Use `mockReturnValue` or `mockResolvedValue` to specify what the mock function should return.
4. **Define custom behavior**: Pass a function to `jest.fn()` to simulate specific logic.
5. **Reset and clear**: Use `mockClear()`, `mockReset()`, and `mockRestore()` to manage the state of mock functions between tests.

By understanding these techniques, you can effectively use `jest.fn()` to create comprehensive unit tests that fully isolate the unit under test while controlling and asserting the behavior of its dependencies.
