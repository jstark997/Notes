# Alternative To Merged Type

For queries that return a schema plus some additional properties a merged type can always be created as a return type for the query function.

Example:

```typescript
export type PostWithData = (Post & { topic: { slug: string }, _count: { comments: number }, user: { name: string}});

export function fetchPostsBySlug(slug: string): Promixe<PostWithData[]> {...}

export function fetchTopPosts(): Promise<PostWithData[]> {...}
```

However in this case there is an alternative.

Example:

```typescript
export type PostWithData = Awaited<ReturnType<typeof fetchPostsBySlug>>[number];
```

The above:

1. Finds the typeof fetchPostBySlug, which is a function.
2. Then looks at what gets returned by the function which is Promise<PostWithData[]>.
3. Then looks at the type wrapped in the Promise, which is PostWithData[].
4. The looks at the type of the first element of the array which is PostWitData.
