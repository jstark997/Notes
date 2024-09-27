# Error Handling In Server Actions

The data in a form is sent from the browser to a server action exectued on the server. However, if there is an error in the form data, that error needs to be communicated from the server action back to the form in the user's browser. To implement this functionality the useFormState hook is used within a client component. Since a server action cannot be defined in a client component it will need to be defined outside of it and imported into it.

## Process with useFormState Hook

1. A client component that contains the form uses the useFormState hook to create a FormState object that is hidden in the form.
2. When the browser submits the form it will send the form data in a FormData object along with the FormState object.
3. The server action that handles the form will receive as arguments both the FormData and the FormState objects.
4. If the server action detects an error, then it will update the FormState object with the error and return it to the browser.
5. When the FormState object is received by the browser the client component containing the form will re-render and display the error in the FormState object.

Example - Server action:

```typescript
'use server';

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

  // Redirect the user back to the root route
  redirect('/');
}
```

In the above the server action receives 2 arguments one for the FormState and the other for the FormData. It performs validation on the title and code propreties within the FormData object and if there is an issue returns a FormState object with the appropriate error message. The server action also has a try/catch block around all the code (except for the call to redirect) so that if there is a runtime error a FormState object will be returned with the error message instead of Next.js displaying a generic error page.

Example - Client component with form

```typescript
'use client';

import { useFormState } from 'react-dom';
import * as actions from '@/actions';

export default function SnippetCreatePage() {
  const [formState, action] = useFormState(actions.createSnippet, {
    message: '',
  });

  return (
    <form action={action}>
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

        {formState.message ? (
          <div className="my-2 p-2 bg-red-200 border rounded border-red-400">
            {formState.message}
          </div>
        ) : null}

        <button type="submit" className="rounded p-2 bg-blue-200">
          Create
        </button>
      </div>
    </form>
  );
}
```

In the above the client component will return JSX for the form. It imports the server action to create the snippet, which also does data validation. The useFormState hook takes the server action and the initial FormState object as arguments and returns the FormState object and a modified server action function with additional functionality. The form action property is assigned the server action function returned by the useFormState hook. The JSX returned includes a section that will display the message in the FormState object if it exists.
