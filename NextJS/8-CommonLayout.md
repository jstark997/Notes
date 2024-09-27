# Common App Layout

The layout.tsx file created when a Next.js app is first set up is where the layout of the UI elements common to all pages in the app is specified.

```
- src/
    - app/
      - layout.tsx
```

When a page is rendered, before it is sent to the client, it is incorporated as a child component in the layout component.

Example:

```typescript
export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <html lang="en">
      <body className={inter.className}>
      <div>
      <ul>
      <li>
        <Link href="/">Home</Link>
      </li>
      <li>
        <Link href="/about">About Us</Link>
      </li>
      <li>
        <Link href="/blog/hello-world">Blog Post</Link>
      </li>
      </ul>
      <\div>
      <div className="container mx-auto px-12">{children}</div>
      </body>
    </html>
  );
}
```

In the above the page links have been placed in the RootLayout component above the children components that are passed to it during the page rendering process.

The \<div\> element that wraps the children components specifies CSS classes that will apply styling to those children components.

The links will now be dispalyed above every page in the app.

**Note** - Be sure to import any components added to the RootLayout.
