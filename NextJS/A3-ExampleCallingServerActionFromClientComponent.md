# Example of Calling a Server Action From a Client Component

**Note** - This example uses the JavaScript bind function to bind the arguments to the server action within the client component.

Example - Project structure:

```
- my-app/
  - .next/
  - node_modules/
  - prisma/
  - public/
  - src/
    - actions/
      - index.ts
    - db/
    - app/
      - snippets
        - [id]/
          - edit/
            - page.tsx
          - page.tsx
        - new/
          - page.tsx
    - components/
      - snippet-edit-form.tsx
```

In the above structure:

- Server component containing client component as a child is at path /snippets/[id]/edit.
- Client componet containing code editor is defined in /components/snippet-edit-form.tsx.
- Server function called by client component is defined in /actions/index.ts.

Example - Server action:

```typescript
'use server';

import { redirect } from 'next/navigation';
import { db } from '@db';

export async function editSnippet(id: number, code: string) {
  await db.snippet.update({
    where: { id },
    data: { code },
  });

  redirect(`/snippets/${id}`);
}
```

In the above the server function takes the id of the code snippet record and the code snippet to update as arguments. It then makes a database call to update the code snippet and then redirects the user to the page displaying the snippet.

Example - Calling server action from within client component:

```typescript
'use client';

import type { Snippet } from '@prisma/client';
import Editor from '@monaco-editor/react';
import { useState } from 'react';
import * as actions from '@/actions';

interface SnippetEditFormProps {
  snippet: Snippet;
}

export default function SnippetEditForm({ snippet }: SnippetEditFormProps) {
  const [code, setCode] = useState(snippet.code);

  const handleEditorChange = (value: string = '') => {
    setCode(value);
  };

  const editSnippetAction = actions.editSnippet.bind(null, snippet.id, code);

  return (
    <div>
      <Editor
        height="40vh"
        theme="vs-dark"
        language="javascript"
        defaultValue={snippet.code}
        options={{ minimap: { enabled: false } }}
        onChange={handleEditorChange}
      />
      <form action={editSnippetAction}>
        <button type="submit" className="p-2 border rounded">
          Save
        </button>
      </form>
    </div>
  );
}
```

In the above the client component imports the server action function. It then binds the server action function arguments using the bind method passing the id of the snippet record (received from the parent server component) and the current state of the code from the editor (managed by the useState hook). When the user submits the form the server action is executed.
