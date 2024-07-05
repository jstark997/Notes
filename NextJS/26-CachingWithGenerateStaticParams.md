# Caching With generateStaticParams

Routes with dynamic path segments are not rendered and cached at build time by Next.js. However, there is a function, 'generateStaticParams', that will, at build time, generate all the possible URLs for a dynamic route and cause the pages for each of those URLs to be rendered and cached.

Example - Project Structure

```
- my-app/
  - .next/
  - node_modules/
  - prisma/
  - public/
  - src/
    - db/
    - app/
      - snippets
        - [id]/
          - page.tsx
        - new/
          - page.tsx
```

Example - Dynamic Route Page

```typescript
import Link from 'next/link';
import { notFound } from 'next/navigation';
import { db } from '@/db';
import * as actions from '@/actions';

interface SnippetShowPageProps {
  params: {
    id: string;
  };
}

export default async function SnippetShowPage(props: SnippetShowPageProps) {
  await new Promise((r) => setTimeout(r, 2000));

  const snippet = await db.snippet.findFirst({
    where: { id: parseInt(props.params.id) },
  });

  if (!snippet) {
    return notFound();
  }

  const deleteSnippetAction = actions.deleteSnippet.bind(null, snippet.id);

  return (
    <div>
      <div className="flex m-4 justify-between items-center">
        <h1 className="text-xl font-bold">{snippet.title}</h1>
        <div className="flex gap-4">
          <Link
            href={`/snippets/${snippet.id}/edit`}
            className="p-2 border rounded"
          >
            Edit
          </Link>
          <form action={deleteSnippetAction}>
            <button className="p-2 border rounded">Delete</button>
          </form>
        </div>
      </div>
      <pre className="p-3 border rounded bg-gray-200 border-gray-200">
        <code>{snippet.code}</code>
      </pre>
    </div>
  );
}

export async function generateStaticParams() {
  const snippets = await db.snippet.findMany();

  return snippets.map((snippet) => {
    return {
      id: snippet.id.toString(),
    };
  });
}
```

In the above at the bottom of the file is an exported function called generateStaticParams that Next.js will call at build time. The function must be named 'generateStaticParams' to be called by Next.js at build time. This function goes through all of the snippets in the database and returns an array of objects with the id of each snippet as a string. Next.js is expecting string values as it then uses those values to create each possible URL for each snippet. Next.js will then render the page for each snippet and cache it at build time. However, when the use edits a code snippet the changes will not be displayed to the user on subsequent requests for the page since only the old cached version will be returned. To solve this problem the server action that implements the updating of a snippet must be changed to refresh the cache with the new data.

Example - Server Action

```typescript
'use server';

import { revalidatePath } from 'next/cache';
import { redirect } from 'next/navigation';
import { db } from '@/db';

export async function editSnippet(id: number, code: string) {
  await db.snippet.update({
    where: { id },
    data: { code },
  });

  revalidatePath(`/snippets/${id}`);
  redirect(`/snippets/${id}`);
}
```

In the above, the server action that updates a code snippet also calls the revalidatePath on the route for that snippet so that the cache is refreshed with a re-rendered page containing the updated code snippet.
