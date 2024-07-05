# Dynamic Paths

## Create

To create dynamic paths use square brackets in the directory name to indicate the dynamic portion of the path.

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
          - page.tsx
        - new/
          - page.tsx
```

In the above 2 paths are defined:

- Dynamic path /users/[id] where '[id]' is the part of the path that will be dynamically populated by the browser.
- Static path /users/new

Example - Component for a dynamic path:

```typescript
export default function UserShowPage(props: any) {
  console.log(props);
  return <div>Show a user</div>;
}
```

In the above the props parameter will be populated by the server with the dynamic parameter of the path.

In this case it will contain the object {params: { id: '1' }}.

Note that Next.js always treats the parts of a path as strings.

## Link

To specify a dynamic path in the href property of a link, place the reference to the dynamic value in the text of the path.

Example:

```typescript
<Link href={`/users/${user.id}/edit`} className="p-2 border rounded">
  Edit
</Link>
```
