# Error Pages

Custom error pages can be created in error.tsx files contained within the directory that defines the route from which the error might be thrown. Next.js will render the component in the error.tsx file whenever an unhandled error is thrown.

Error page components:

- Must be client components.
- Will be passed an object with 2 properties: 'error' and 'reset'.
- The error property will have the Error object thrown.
- The reset property will have a function that can be used to automatically refresh the route.

Example - error.tsx page:

```typescript
'use client';

interface ErrorPageProps {
  error: Error;
  reset: () => void;
}

export default function ErrorPage({ error }: ErrorPageProps) {
  return <div>{error.message}</div>;
}
```

In the above the error page component just returns a \<div\> element that displays the error message.

**Note** - An error page component such as the example above is probably not the best way to handle errors as it will display to the user a very simple error page from which it will not be obvious what the user should do next.
