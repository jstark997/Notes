# Static and Dynamic Routes

- **Static** route is one that only contains static (unchanging) data.
- **Dynamic** route is one that contains data that might change.

## Build Process Output

When the 'npm run build' command is executed to build a Next.js app, after the build have completed successfully there will be output that shows which routes Next.js determined were static or dynamic. The output will be a tree showing the different routes and next to each route will be a symbol. If the symbol next to the route is a circle, then Next.js determined that route to be static. If the symbol next to a route is the Greek letter lambda, then Next.js determined the route is dynamic.

## How Next.js Determines Whether a Route is Static or Dynamic

Routes are determined to be **static** by default.

Routes can be determined to be **dynamic** if:

- A dynamic function is called or a dynamic variable is referenced when the route renders.
- Specific route segment config options have been assigned.
- 'Fetch' is called and caching of the response has been opted out of.
- The path of the route contains a dynamic segment.

### Dynamic Function Call And Dynamic Variable Reference

- Modifiying cookies with cookie.set() or cookie.delete().
- Doing anything involving a query string.

### Route Segment Config Options

1. force-dynamic

```typescript
export const dynamic = 'force-dynamic';
```

2. revalidate

```typescript
export const revalidate = 0;
```

Place either of the above lines below the import statements in the component for the route.

### Fetch Options

```typescript
fetch('...', { next: { revalidate: 0 } });
```

Call fetch with option to not cache response.

### Dynamic Path

Example:

```sh
/snippets/[id]/page.tsx
```
