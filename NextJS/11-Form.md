# Form

Forms are used to enter data into a web application.

Example:

```typescript
export default function SnippetCreatePage() {
  return (
    <form>
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

The default behavior of the submit action of a form is to make a request to the backend server with a query string that contains the data of the form.

```
localhost:3000/snippets/new?title=ABC%20function&code=const%20abc%20%3D%20%28%29%20%3D%3E%20%7B%7D
```

In the above the data from the title and code input controls are used to create the query string that is part of the request to the backend server.

The action property of a form is commonly set to a Next.js server action to enable the application to change data based the user input to the form.
