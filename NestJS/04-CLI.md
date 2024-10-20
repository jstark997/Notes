# Command Line Inteface

The NestJS CLI simplifies the process of creating new components and organizes the application code efficiently. By default, when you use the CLI to generate a new component, it will create a separate directory for each generated controller, service, or other components within the related module's folder. However, you can use the `--flat` flag to generate components without creating a new directory.

Below are some common commands:

- **Generate a module**:

  ```bash
  nest generate module users
  ```

  This creates a new module file under `src/users/`.

- **Generate a controller**:

  ```bash
  nest generate controller users
  ```

  This creates a new directory `src/users/` with a controller file `users.controller.ts` inside it.

  - **With the `--flat` flag**:
    ```bash
    nest generate controller users --flat
    ```
    This creates the `users.controller.ts` file directly inside `src/users/`, without generating a separate directory for the controller.

- **Generate a service**:

  ```bash
  nest generate service users
  ```

  This creates a new directory `src/users/` with a service file `users.service.ts`.

  - **With the `--flat` flag**:
    ```bash
    nest generate service users --flat
    ```
    This creates the `users.service.ts` file directly inside `src/users/`, without generating a separate directory for the service.

In summary, the default behavior of the CLI is to generate a new directory for each component (like controllers or services) to help keep the codebase modular and organized. However, the `--flat` flag gives you control over the structure, allowing you to place the generated files directly within the module's folder without additional subdirectories.
