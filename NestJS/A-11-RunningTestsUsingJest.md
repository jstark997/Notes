# Running Tests Using Jest

Running unit tests in the terminal using Jest is straightforward. Here’s how you can do it:

### 1. **Running All Tests**

To run all tests in your project, you can use the following command:

```bash
npx jest
```

If you have Jest installed globally, you can also use:

```bash
jest
```

### 2. **Running Tests in Watch Mode**

If you want Jest to watch for file changes and rerun the tests automatically, you can add the `--watch` option:

```bash
npx jest --watch
```

### 3. **Running Specific Tests**

You might want to run a specific test file or a subset of your tests. Jest provides multiple ways to do this:

#### a. **Run a Specific Test File**

To run a specific test file, provide the path to the file:

```bash
npx jest path/to/your/test/file.spec.ts
```

For example:

```bash
npx jest src/user/user.service.spec.ts
```

#### b. **Run Tests by Name**

Jest allows you to run tests that match a specific name or pattern using the `-t` or `--testNamePattern` option. This is useful if you only want to run a single test or a set of tests that have similar names.

```bash
npx jest -t "should return user when user is found"
```

In this example, Jest will only run the test cases that include `"should return user when user is found"` in their names.

#### c. **Run Tests in a Specific Directory**

To run all the tests within a specific directory, specify the directory:

```bash
npx jest src/user
```

### 4. **Filtering Tests Using Patterns**

Jest can also run tests that match specific filename patterns. For example, if you want to run only the tests related to `service` files:

```bash
npx jest --testPathPattern=service
```

This will run all test files that have "service" in their path or filename.

### 5. **Additional Useful Jest Options**

- **`--coverage`**: To run tests and collect code coverage information:

  ```bash
  npx jest --coverage
  ```

- **`--verbose`**: To get detailed information about each test run:

  ```bash
  npx jest --verbose
  ```

- **`--bail`**: Stop running tests after the first failure:

  ```bash
  npx jest --bail
  ```

### Example Workflow

1. **Run all tests**:

   ```bash
   npx jest
   ```

2. **Run a specific test file**:

   ```bash
   npx jest tests/api/user.service.spec.ts
   ```

3. **Run a specific test by name**:

   ```bash
   npx jest -t "should handle missing parameters"
   ```

4. **Run tests in watch mode for a specific file**:

   ```bash
   npx jest src/user/user.service.spec.ts --watch
   ```

These commands help streamline your development process by allowing you to run and debug your unit tests more efficiently.

### `--maxWorkers` Flag

The `--maxWorkers` flag in Jest allows you to control how many test files are run in parallel. By default, Jest will run tests in parallel using as many workers (processes) as there are CPU cores on your machine. However, if you want to limit the number of tests running concurrently, you can use the `--maxWorkers` option to specify the number of workers.

### Running Tests One at a Time

To run tests one at a time, you can set `maxWorkers` to 1. This ensures that Jest only runs one test file at a time instead of running multiple tests in parallel.

```bash
npx jest --maxWorkers=1
```

This will make Jest run the tests sequentially, one after another. This is particularly useful when:

- Your tests depend on shared resources that can lead to race conditions.
- You are troubleshooting or debugging and want tests to run slowly for better tracking.

### How to Use the `--maxWorkers` Flag

1. **Run Tests One at a Time:**

   ```bash
   npx jest --maxWorkers=1
   ```

2. **Run Tests with Limited Workers:**

   If you want to limit the number of workers to something other than one (e.g., 2 workers running in parallel), you can adjust the flag:

   ```bash
   npx jest --maxWorkers=2
   ```

This flexibility allows you to control how much parallelism Jest uses, based on your testing or resource needs.

### Example Use Cases for `maxWorkers=1`:

- **Troubleshooting Flaky Tests**: Some tests may fail intermittently due to parallel execution issues, and running them sequentially can help pinpoint problems.
- **Heavy Resource Utilization**: If each test consumes a lot of resources (e.g., database connections or memory), limiting parallelism can help avoid overloading your system.
- **Debugging**: Running tests one by one can simplify debugging, especially when combined with `--verbose` for detailed output.

Using `--maxWorkers=1` ensures the tests are executed sequentially, providing a controlled environment for test execution.
