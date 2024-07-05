# Authentication

The following authentication examples use OAuth for authentication.

## Sign In

To allow the user to sign in call the server action that wraps the sign in function as an action for a form.

```typescript
import * as actions from '@/actions';

export default function Page() {
  return (
    <form action={actions.signIn}>
      <button type="submit">Sign In</button>
    </form>
  );
}
```

## Sign Out

To allow the user to sign out call the server action that wraps the sign out function as an action for a form.

```typescript
import * as actions from '@/actions';

export default function Page() {
  return (
    <form action={actions.signOut}>
      <button type="submit">Sign Out</button>
    </form>
  );
}
```

## Check If a User is Signed In From Server Component

To check if a user is signed call the auth function (defined in auth.ts in the project) to get the session object, and check if the user property is set.

```typescript
import { auth } from '@auth';

export default async function Page() {
  const session = await auth();

  if (session?.user) {
    return <div>Signed in</div>;
  } else {
    return <div>Signed out</div>;
  }
}
```

**Note** - The auth function returns either a Session object or null. If a Session object is returned then it will have a user property set with the user's information if the user is signed in.

## Check If a User is Signed In From Client Component

In order for this to work, need to set up a SessionProvider in the provider.tsx file. The SessionProvider uses the React context system to share session information with all of the client components in the application.

To set up a SessionProvider, in providers.tsx in the src/app directory wrap the NextUIProvider in a SessionProvider:

```typescript
'use client';

import { NextUIProvider } from '@nextui-org/react';
import { SessionProvider } from 'next-auth/react';

interface ProvidersProps {
  children: React.ReactNode;
}

export default function Providers({ children }: ProvidersProps) {
  return (
    <SessionProvider>
      <NextUIProvider>{children}</NextUIProvider>
    </SessionProvider>
  );
}
```

To check if a user is signed in use the useSession hook from next-auth/react to get the session information and check if it contains a user object:

```typescript
'use client';

import { useSession } from 'next-auth/react';

export default afunction Profile() {
  const session = useSession();

  if (session.data?.user) {
    return <div>Signed in</div>
  } else {
    return <div>Signed out</div>
  }
}
```

**Note** - The useSession hook return an object that is always defined. If the user is signed in this object will have a data property that will have a user property with the user's information.

## Complete Example

### Profile

```typescript
'use client';

import { useSession } from 'next-auth/react';

export default function Profile() {
  const session = useSession();

  if (session.data?.user) {
    return <div>From client: {JSON.stringify(session.data.user)}</div>;
  }

  return <div>From client: user is NOT signed in</div>;
}
```

### Home Page

```typescript
import { Button } from '@nextui-org/react';
import * as actions from '@/actions';
import { auth } from '@/auth';
import Profile from '@/components/profile';

export default async function Home() {
  const session = await auth();

  return (
    <div>
      <form action={actions.signIn}>
        <Button type="submit">Sign In</Button>
      </form>

      <form action={actions.signOut}>
        <Button type="submit">Sign Out</Button>
      </form>

      {session?.user ? (
        <div>{JSON.stringify(session.user)}</div>
      ) : (
        <div>Signed Out</div>
      )}

      <Profile />
    </div>
  );
}
```
