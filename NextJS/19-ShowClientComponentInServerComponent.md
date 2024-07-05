# Showing a Client Component in a Server Component

Since server components can not use hooks or assign event handlers, to make use of that React functionality requires client components. Server components can contain client components as children and pass properties to them.

Example - Client Component:

```typescript
'use client';

import type { Snippet } from '@prisma/client';
import Editor from '@monaco-editor/react';
import { useState } from 'react';

interface SnippetEditFormProps {
  snippet: Snippet;
}

export default function SnippetEditForm({ snippet }: SnippetEditFormProps) {
  const [code, setCode] = useState(snippet.code);

  const handleEditorChange = (value: string = '') => {
    setCode(value);
  };

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
    </div>
  );
}
```

In the above a client component is created to provide code snippet editing functionality using a third party editor that requires the use of hooks and event handlers. This component will receive the code snippet record to edit as a property from the parent server component. Note the use of the 'use client' directive. This client component is rendered on the server.

Example - Server Component:

```typescript
import { notFound } from 'next/navigation';
import { db } from '@/db';
import SnippetEditForm from '@/components/snippet-edit-form';

interface SnippetEditPageProps {
  params: {
    id: string;
  };
}

export default async function SnippetEditPage(props: SnippetEditPageProps) {
  const id = parseInt(props.params.id);
  const snippet = await db.snippet.findFirst({
    where: { id },
  });

  if (!snippet) {
    return notFound();
  }

  return (
    <div>
      <SnippetEditForm snippet={snippet} />
    </div>
  );
}
```

In the above the server component contains as a child a client component that it passes data to as a property.
