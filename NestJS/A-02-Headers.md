# The `@Headers()` Annotation in NestJS

The `@Headers()` annotation in NestJS is used to access the HTTP headers from an incoming request. This decorator allows you to retrieve individual header values or all headers sent with the request. HTTP headers contain metadata about the request or the client, such as content type, authorization tokens, user agent information, and more.

#### Usage

1. **Extract All Headers**: By using `@Headers()` without any arguments, you can extract all the headers as an object.
2. **Extract Specific Header**: By passing a string argument to `@Headers()`, you can extract a specific header by name.

#### Example 1: Extract All Headers

```typescript
import { Controller, Get, Headers } from "@nestjs/common";

@Controller("users")
export class UsersController {
  @Get("info")
  getUserInfo(@Headers() headers: Record<string, string>) {
    console.log(headers);
    return `Headers received: ${JSON.stringify(headers)}`;
  }
}
```

In this example:

- The `@Headers()` decorator is used to extract all HTTP headers from the incoming request.
- The `headers` parameter receives a key-value object where each key is a header name and each value is the corresponding header value.
- The `getUserInfo()` method logs and returns all headers sent with the request.

When a request is made to `/users/info`, all the headers will be logged and returned in the response.

#### Example 2: Extract a Specific Header

```typescript
import { Controller, Get, Headers } from "@nestjs/common";

@Controller("users")
export class UsersController {
  @Get("info")
  getUserAgent(@Headers("user-agent") userAgent: string) {
    console.log(userAgent);
    return `User-Agent: ${userAgent}`;
  }
}
```

In this example:

- The `@Headers('user-agent')` decorator is used to extract only the `User-Agent` header from the incoming request.
- The `userAgent` parameter contains the value of the `User-Agent` header.
- The method logs and returns the `User-Agent` value.

When a request is made to `/users/info`, the server will log and return the client's user agent (e.g., browser or app information).

#### Conclusion

The `@Headers()` decorator is a useful tool for accessing HTTP headers in NestJS. It can either retrieve all headers from the request or fetch specific ones by name. This is commonly used for tasks like retrieving authentication tokens, checking content types, or examining client information (e.g., `User-Agent`).
