# Where To Fetch Data

Pages often have many, possibly nested child components. This brings up a question as to in which component the data for the page should be fetched from the database. That is should data fetching occur higher up or lower down in the component hierarchy for the page?

Three options:

1. Page component fetches the data and passes it to the child component(s).
2. Child component fetches its own data.
3. Hybrid approach- parent component decides what data to fetch, but childern fetch it.

## Option 1 - Page Fetches Data

### Advantages

- Easy to see what data a route needs to display.
- Easier to make child component reusable.
- Easier to avoid 'n+1' query issues.

### Disadvantages

- Can _sometimes_ lead to overfetching of data.
- Can _sometimes_ lead to duplicate code in other pages using the child component.
- _Sometimes_ annoying to write the interface for complex query data.
- _Slightly_ slower page loading.

## Option 2 - Child Fetches Data

### Advantages

- Easier to build skeleton loading pages, that is pages with placeholders for data that is still loading.

### Disadvantages

- Child component implementation is locked in.

## Option 3 - Hybrid Approach

### Step 1

Create a separate file with all the queries to retrieve the data for reusalbe child components. Each query returns the same data type, which depending on the query may have properties that are null.

Example:

```typescript
export type PostWithData = (Post & { topic: { slug: string }, _count: { comments: number }, user: { name: string}});

export function fetchPostsBySlug(slug: string): Promixe<PostWithData[]> {...}

export function fetchTopPosts(): Promise<PostWithData[]> {...}
```

### Step 2

Define an interface with a property that will accept one of the query functions. This interface will be passed to the child component in the props argument.

Example - Interface:

```typescript
interface PostListProps {
  fetchData: () => Promise<PostWithData[]>;
}
```

Example - Reusuable child component:

```typescript
export default funciton async PostList({ fetchData }: PostListProps) {
  const posts = await fetchData();
  return posts.map(() => ...);
}
```

### Step 3

Parent component can decide which query it needs and pass the funciton for that query to the reusable child component in props.

Example 1:

```typescript
import { fetchPostsBySlug } from 'queries/posts';
import PostLilst from './post-list';

export default function TopicShowPage({ params: { slug }}) {
  return <PostList fetchData={()= > fetchPostsBySlug(slug)} />
}
```

Example 2:

```typescript
import { fetchTopPosts } from 'queries/posts';
import PostLilst from './post-list';

export default function HomePage({ params: { slug }}) {
  return <PostList fetchData={()= > fetchTopPosts} />
}
```
