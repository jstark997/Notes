# Linking Pages

Next.js has a Link component for creating links between pages.

This component extends the HTML \<a\> element and is the primary way to navigate between routes in Next.js.

Example:

```typescript
import Link from 'next/link';

function Home() {
  return (
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
  );
}

export default Home;
```
