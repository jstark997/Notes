# Request Memoization

Next.js has a built-in caching memoization system that 'deduplicates' requests made for the same data from, for example, HTTP or database requests.

Some things to keep in mind:

- The cache memoization system is cleared out between incoming requests. (Does not need to be managed.)
- Automatically used wit the built-in 'fetch' function.
- Can be used with other functions (like db queries) by using the '**cache**' function.

Example - Database query:

```typescript
import type { Comment } from '@prisma/client';
import { cache } from 'react';
import { db } from '@/db';

export type CommentWithAuthor = Comment & {
  user: { name: string | null; image: string | null };
};

export const fetchCommentsByPostId = cache(
  (postId: string): Promise<CommentWithAuthor[]> => {
    console.log('Making a query!');

    return db.comment.findMany({
      where: { postId },
      include: {
        user: {
          select: {
            name: true,
            image: true,
          },
        },
      },
    });
  }
);
```

In the above the function that executes the database query is passed to the cache function to be memoizaed. Everytime 'fetchCommentsByPostId' is called in the application with the same argument the cache memoization system will actually only call the function once and then pass the data to all the other calls to the function with the same argument.
