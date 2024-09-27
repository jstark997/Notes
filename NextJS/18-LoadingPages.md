# Loading Pages

Next.js will diplay a loading page if the directory of the path for the page contains a loading.tsx file.

**Note** - It is not necessary to create a loading.tsx file to create a loading spinner. That functionality can be implemented directly in the component. The loading.tsx is just a convenience.

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
          - loading.tsx
          - page.tsx
        - new/
          - page.tsx
```

Example - Loading page:

```typescript
export default function UserLoadingPage() {
  return <div>Loading...</div>;
}
```
