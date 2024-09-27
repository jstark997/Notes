# Fetching A Specific Record

Example:

```typescript
import { notFound } from 'next/navigation';
import { db } from '@/db';

interface SnippetShowPageProps {
  params: {
    id: string;
  };
}

export default async function SnippetShowPage(props: SnippetShowPageProps) {
  const snippet = await db.snippet.findFirst({
    where: { id: parseInt(props.params.id) },
  });

  if (!snippet) {
    return notFound();
  }

  return <div>{snippet.title}</div>;
}
```

**Notice:**

- An interface is created with the structure of the expected properties passed to the component.
- The findFirst method is used to selectg a specific record by id.
- The params.id property which is passed to the the component as a string is parsed as an integer which is what the findFirst method expects.
- The findFirst method could return null if no record with the passed id is found.
- The component handles the null case by calling the Next.js navigation notFound function.
