# Custom Not Found Pages

Next.js provides in the navigation module a notFound function that when called will by default display a generic 404 not found page, unless a custom page is available.

**Note** - It is not necessary to use the notFound function. The same functionality can be acheived within the component by returning JSX directly when data is not found.

To create a custom not found page for a path, create a file named not-found.tsx in the directory the defines the path with the content to be displayed.

Example:

```
- my-app/
  - .next/
  - node_modules/
  - prisma/
  - public/
  - src/
    - db/
    - app/
      - users
        - [id]/
          - not-found.tsx
          - page.tsx
        - new/
          - page.tsx
```

Example - Custom not found page:

```typescript
export default function UserNotFound() {
  return (
    <div>
      <h1 className="text-xl bold">Sorry, but could not find that user</h1>
    </div>
  );
}
```

**Note** - When the component in the page.tsx file calls the notFound function the server will look for the closest not-found.tsx file and execute it.
