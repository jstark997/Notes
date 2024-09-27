# Caching

Next.js has 4 caches:

1. Data Cache - Responses from requests made with 'fetch' stored and used across requests.
2. Router Cache - 'Soft' navigation between routes are cached in the browser and reused when a user revisits a page.
3. Request Memoization - If two or more 'GET' requests are made with 'fetch', then only one is actually executed.
4. Full Route Cache - During the build prcess Next renders and caches pages for routes it determines are static and these are served to users when the app is run in production.

## Data Cache

## Route Cache

## Request Memoization

## Full Route Cache

### Build Process

1. During the build process Next.js finds all the routes of the app.
2. For each route Next.js uses certain criteria to determine if the route is static or dynamic.
3. If Next.js determines that a route is static then it renders the page during the build and caches it.

## Cache Controls

There are 3 ways to control caching:

1. Time Based - After a set amount of time, ignore teh cached response and fetch new data.
2. On-Demand - Refresh the cache with new data based on some user input or condition.
3. Disable Cahcing - Disallow any caching.

### Time Based Control

Example:

```typescript
export const revalidate = 3;

export default async function Page() {
  const snippets = await db.snippets.findMany();

  return (
    <div>{snippets.map(...)}</div>
  )
}
```

In the above the route segment config option 'revalidate' is set to 3, which is interpretted by Next.js as 3 seconds. So after 3 seconds, the next request for the page will cause the page to re-render and that page will be cached for the next 3 seconds.

Use case: Front end of a social media site where data changes often.

### On-Demand Control

Example:

```typescript
import { revalidatePath } from 'next/cache';

//When there is some user input or condition
//that determines the data for the '/snippets' route has changed
//then call ...
revalidatePath('/snippets');
```

In the above the revalidatePath function is called on a route when triggered by some user input or some condition being satisfied. This causes the Next.js to remove the page from the cache and re-render it upon the next request.

Use case: When the condition under which data changes is known or can be predicted.

### Disable Caching

Example - Set revalidate option to 0

```typescript
export const revalidate = 0;

export default async function Page() {
  //Never caches
}
```

Example - set 'force-dynamic' option

```typescript
export const dynamic = 'force-dynamic';

export default async function Page() {
  //Never caches
}
```

The above options disable caching for a route.

Use cases:

- When the conditions under which data changes are unknown (for example making an call to an outside API).
- When it is expected that the data will change with every request.
