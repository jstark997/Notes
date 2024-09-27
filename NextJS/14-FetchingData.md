# Fetching Data

To create a component that fetches data:

1. Create a server component (the default in Next.js).
2. Mark the component as 'async'.
3. Request the data: HTTP request, database call, etc.
4. Render the data directly or pass it to a child component.

Example:

```typescript
import { db } from '@/db';

export default async function Home() {
  const snippets = await db.snippet.findMany();

  const renderedSnippets = snippets.map((snippet) => {
    return <div key={snippet.id}>{snippet.title}</div>;
  });

  return <div>{renderedSnippets}</div>;
}
```

In the above all the records from the database are fetched and the title property for those records are returned as an array.

Notice that the component function is declared as 'async' allowing the use of 'await' inside the component for the call to the database.
