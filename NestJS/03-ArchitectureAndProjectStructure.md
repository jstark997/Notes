# Architecture and Project Structure

NestJS follows the **Modular Architecture** principle, which divides an application into multiple modules that can be independently managed and loaded. Here's an overview of the architecture:

- **Modules**: The basic building block, used to group related components such as services, controllers, and providers.
- **Controllers**: Handle incoming HTTP requests, routing them to the appropriate services.
- **Providers**: Typically services or helpers, used to handle business logic and provide data.
- **Middleware**: Functions that are executed before the request reaches the controller.
- **Guards**: Used to implement authentication and authorization.
- **Pipes**: Handle data validation and transformation.
- **Interceptors**: Modify incoming requests or outgoing responses.

A typical project structure might look like this:

```
src/
  ├── app.module.ts        # The root module
  ├── app.controller.ts    # Controller for handling requests
  ├── app.service.ts       # Service containing business logic
  └── main.ts              # Entry point
```
