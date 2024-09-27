# File-Based Routing

Files and directories placed in the src/app directory determine the routes in a Next.js app:

- Files named 'page.tsx' placed in the src/app directory or sub-directories define a route in the app.
- The content of the 'page.tsx' file must contain a React component that is exported with the 'default' keyword.
- The names of the sub-directories in the src/app directory define the paths of the routes in the app.

The following creates a page for the root route '/':

```
- src/
    - app/
      - page.tsx
```

Example of page.tsx contents:

```typescript
export default function HomePage() {
  return <h1>Home Page</h1>;
}
```

To define a route:

1. Create a directory in src/app with the name of the route.
2. In this directory create a page.tsx file with the code of the React component for the page at that route.

```
- src/
    - app/
      - about/
        - page.tsx
      - page.tsx
```

Nested routes can be created using sub-directories:

```
- src/
    - app/
      - about/
        - details/
          - page.tsx
        - page.tsx
      - page.tsx
```

The above has 3 routes:

- '\\'
- '\about'
- '\about\details'
