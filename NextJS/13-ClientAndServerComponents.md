# Client And Server Components

There are 2 general types of components in a Next.js app:

1. Client components
2. Server components

## Client Components

- Returns JSX.
- Can use all the features available in React.
- In Next.js client components are defined with the directive 'use client' place at the top of the file for the component.
- In general can not directly show a server component (a server component can not be contained with the JSX returned).
- _Use client components if hooks or event handlers are required_.

## Server Components

- Preferred over client components, whenever possible, as takes full advantage of Next.js providing better performance and UX.
- Returns JSX.
- Is the default component in Next.js.
- Can use async/await directly in the component for data fetching, instead of useState or useEffect hooks.
- Don't need to use third party state management libraries such as Redux.
- **No hooks** (such as useState) are allowed.
- **No event handlers** (such as for onClick) can be assigned within the component.

## Rendering Process

Example for a server component showing a client component:

1. Browser makes a request to the Next.js server.
2. Server receives request.
3. HTML for components is rendered on the server
4. Server returns HTML immediately.
5. HTML returned will have a script tag in it that causes the browser to download from the server the JavaScript associated with the client component.
6. Server receives the second request from the browser.
7. Server looks at the client component and renders the JavaScript required to implement any event handlers or state.
8. Server returns the JavaScript to the browser as a file.
9. Browser executes the JavaScript.

Everything, HTML and JavaScript, gets rendered once on the server and then returned to the browser.
