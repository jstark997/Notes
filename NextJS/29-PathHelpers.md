# Path Helpers

Beacuse Next.js uses file based routing a large project can have many paths implemented by deeply nested directories. If those paths are referenced as strings throughout the project, then if the project structure changes all the path strings would need to be changed wherever they are used. This could end up being a time consuing and error prone process.

Therefore it is recommended to create a set of functions called **path helpers** in a single file that can be called whenever the string reference for a path is needed. Then if the project structure changes only the contents of a single file needs to be changed.

Example - Project Structure:

```
- dis-app/
  - src/
    - app/
      - topics/
        - [slug]/
          - posts/
            - new/
              - page.tsx
            -[postId]
              - page.tsx
          - page.tsx
    - path.ts
```

Exmple - Path helpers in paths.ts file:

```typescript
const paths = {
  home() {
    return '/';
  },
  topicShow(topicSlug: string) {
    return `/topics/${topicSlug}`;
  },
  postCreate(topicSlug: string) {
    return `/topics/${topicSlug}/posts/new`;
  },
  postShow(topicSlug: string, postId: string) {
    return `/topics/${topicSlug}/posts/${postId}`;
  },
};

export default paths;
```

Exmaple - Calling a path helper:

```typescript
<Link href={paths.postCreate(topic.slug)}>Create</Link>
```
