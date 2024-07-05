# Server Actions

Operations executed on the server in a Next.js app:

- Most common way to change data in the app
- Close integration with HTML forms
- Are functions called with data entered into a form

Execution:

1. Server running Next.js app receives form data.
2. Next passes form data to the server action.
3. The function that is the server action is called on the data.
4. Server returns a response to the client.

## Defining

The function that implements a server action must contain the directive string 'use server' within it.

**Note** - A server action **cannot** be defined within a client component.

Example - Creating a Snippet record

```typescript
import { redirect } from 'next/navigation';
import { db } from '@/db';

export default function SnippetCreatePage() {
  async function createSnippet(formData: FormData) {
    // This needs to be a server action!
    'use server';

    // Check the user's inputs and make sure they're valid
    //FormData.get method returns type FormDataEntry | null which here is being cast as string
    const title = formData.get('title') as string;
    const code = formData.get('code') as string;

    // Create a new record in the database
    const snippet = await db.snippet.create({
      data: {
        title,
        code,
      },
    });
    console.log(snippet);

    // Redirect the user back to the root route
    redirect('/');
  }

  return (
    <form action={createSnippet}>
      <h3 className="font-bold m-3">Create a Snippet</h3>
      <div className="flex flex-col gap-4">
        <div className="flex gap-4">
          <label className="w-12" htmlFor="title">
            Title
          </label>
          <input
            name="title"
            className="border rounded p-2 w-full"
            id="title"
          />
        </div>

        <div className="flex gap-4">
          <label className="w-12" htmlFor="code">
            Code
          </label>
          <textarea
            name="code"
            className="border rounded p-2 w-full"
            id="code"
          />
        </div>

        <button type="submit" className="rounded p-2 bg-blue-200">
          Create
        </button>
      </div>
    </form>
  );
}
```

## Defining for Use Within a Client Component

There are 2 options to enable a client component to make use of a server action:

1. Define the server action in a server component and pass it in the properites passed to the client component.
2. Define the server action in a file separate from the server component and have the client component import the server action function.

### Option 1 - Passing server action to Client Component in props

Note - In general server components cannot pass event handlers to client components, but server actions are the exception.

Example:

```typescript
export async function SnippetEditPage() {
  async function updateSnippet(id, code) {
    'use server';

    //Code to update the snippet
  }

  return (
    <div>
      <SnippetEditForm onSubmit={updateSnippet} />
    </div>
  );
}
```

### Option 2 - Defining Server Action in a Separate File

Example - Create a a directory actions with file index.ts to store server actions:

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
          -edit/
            - page.tsx
          - page.tsx
        - new/
          - page.tsx
```

Example - In index.ts create the server action function:

```typescript
'use server';

import { db } from '@db';

export async function updateSnippet() {
  //Code to update a snippet
}
```

Note the 'use server' directive at the top of the file. It will make all functions declared in this file server actions.
Note that the file does not need to be named index.ts, but it can be convenient to do so when importing the the functions. However, if the server actions end up including a lot of code, then it is probably best to put them in separate files. But then those server actions can still be imported and then exported within the index.ts file to make them convenient to use elsewhere.

To make use of the server action the client component needs to import the server action function defined in the index.ts file.

## Calling a Server Action Within a Client Component

There a 2 options for calling a server action within a client compoent:

1. Use the bind function to bind the arguments to the server action.
2. Use the startTransition function from React.

### Option 1 - Bind the Arguments

Example - Server action:

```typescript
'use server';

export async function updateSnippet(code: string, formData: FormData) {
  //Code to update the snippet
}
```

In the above the server action is defined with 2 arguments, one for the code snippet and the other for any data from the form that the function will be called from (if any).

Example - Bind the arguments in the client component:

```typescript
'use client'

import * as actions from 'actions';

export default function SnippetEditForm() {
  const [code, setCode] = useState('');

  //Make sure 'updateSnippet' gets called with the code from the editor
  const updateSnippetAction = actions.updateSnippet.bind(null, code);

  return (
    <form action={updateSnippetAction}>
      <button type="submit">Submit</button>
    <\form>
  );
}
```

In the above the client component imports the server function and then binds the first argument to the current state of the code in the editor (managed by the useState hook). The bound server function is then assigned to the to action property of the form. In this case there is no form data to pass to the server action.

**Advantages**:

- Form will work even if the user's browser is not running JavaScript (because, for example, it has been disabled).
- Next.js documentation favors this approach.

### Option 2 - Use startTransition

Example - Server action:

```typescript
'use server';

export async function updateSnippet(code: string) {
  //Code to update the snippet
}
```

In the above the server action is defined with 1 argument for the code snippet.

Example - Use startTransition function:

```typescript
'use client';

import { startTransition } from 'react';
import * as actions from 'actions';

export default function SnippetEditForm() {
  const [code, setCode] = useState('');

  const handleClick = () => {
    //Make sure the data is updated before any attempt to navigate
    startTransition(async () => {
      await actions.updateSnippet(code);
    });
  };

  return <button onClick={handleClick}>Submit</button>;
}
```

Server actions typically create or change data and then redirect the user somewhere else. The startTransition function synchronizes the change in data with the navigation to another part of the application.

**Advantages**:

- Server action can be run at anytime, not tied to a use submitting a form.
- Does not require using the bind function.
- Closer to classic React behavior.
