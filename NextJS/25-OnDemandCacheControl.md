# On-Demand Cache Control

Cache is refreshed with a re-rendered page when a condition indicating a change in the data is met.

Example - Home Page:

```typescript
import Link from 'next/link';
import { db } from '@/db';

export default async function Home() {
  const snippets = await db.snippet.findMany();

  const renderedSnippets = snippets.map((snippet) => {
    return (
      <Link
        key={snippet.id}
        href={`/snippets/${snippet.id}`}
        className="flex justify-between items-center p-2 border rounded"
      >
        <div>{snippet.title}</div>
        <div>View</div>
      </Link>
    );
  });

  return (
    <div>
      <div className="flex m-2 justify-between items-center">
        <h1 className="text-xl font-bold">Snippets</h1>
        <Link href="/snippets/new" className="border p-2 rounded">
          New
        </Link>
      </div>
      <div className="flex flex-col gap-2">{renderedSnippets}</div>
    </div>
  );
}
```

The above is a component that renders a Home page for an app that creates, edits and deletes code snippets stored in a database. The Home page displays the title of the code snippets stored. When the app is built Next.js will determine that the Home page is a static route and will render and cache the Home page during the build process. In order to handle the cases where a code snippet is created or deleted, on demand cahce control can be implemented in the server actions that implement creation and deletion.

Eamcple - Server Actions:

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

  redirect(`/snippets/${id}`);
}

export async function deleteSnippet(id: number) {
  await db.snippet.delete({
    where: { id },
  });

  revalidatePath('/');
  redirect('/');
}

export async function createSnippet(
  formState: { message: string },
  formData: FormData
) {
  try {
    // Check the user's inputs and make sure they're valid
    const title = formData.get('title');
    const code = formData.get('code');

    if (typeof title !== 'string' || title.length < 3) {
      return {
        message: 'Title must be longer',
      };
    }
    if (typeof code !== 'string' || code.length < 10) {
      return {
        message: 'Code must be longer',
      };
    }

    // Create a new record in the database
    await db.snippet.create({
      data: {
        title,
        code,
      },
    });
  } catch (err: unknown) {
    if (err instanceof Error) {
      return {
        message: err.message,
      };
    } else {
      return {
        message: 'Something went wrong...',
      };
    }
  }

  revalidatePath('/');
  // Redirect the user back to the root route
  redirect('/');
}
```

In the above the server actions that handle creation and deletion of code snippets call the revalidatePath function on the route for the Home page causing Next.js to clear the cache, re-render the page and cache the newly rendered page. Note that revalidatePath is not called in the server action that updates a code snippet since that does not affect what is displayed in the Home page. Using on-demand cache control in this instance improves performance since a cached version of the Home page is served unless a change has occurred that affects what it displays.
