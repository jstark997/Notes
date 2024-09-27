# Static Caching With Dynamic Components

If the page of a route has a dynamic component (a component that calls a dynamic function, sets a dynamic variable, sets a route segement config option or calls fetch), then Next.js will identify that route as a dynamic route and will not cache it statically, even if the main content of the page is static.

For example, a Header component that calls a server action to check if a user is authenticated by checking the user property of a cookie. In this example, one solution is to separate the authentication functionality of the Header component into a separate client component that uses the useSession hook to access the cookie. Beacuse the useSession hook does not directly access cookies (it makes a request to the backend to get the data), Next.js will not consider it a dynamic function.

Example - HeaderAuth client component:

```typescript
'use client';

import Link from 'next/link';
import {
  Navbar,
  NavbarBrand,
  NavbarContent,
  NavbarItem,
  Input,
  Button,
  Avatar,
  Popover,
  PopoverTrigger,
  PopoverContent,
} from '@nextui-org/react';
import { useSession } from 'next-auth/react';
import * as actions from '@/actions';

export default function HeaderAuth() {
  const session = useSession();

  let authContent: React.ReactNode;
  if (session.status === 'loading') {
    authContent = null;
  } else if (session.data?.user) {
    authContent = (
      <Popover placement="left">
        <PopoverTrigger>
          <Avatar src={session.data.user.image || ''} />
        </PopoverTrigger>
        <PopoverContent>
          <div className="p-4">
            <form action={actions.signOut}>
              <Button type="submit">Sign Out</Button>
            </form>
          </div>
        </PopoverContent>
      </Popover>
    );
  } else {
    authContent = (
      <>
        <NavbarItem>
          <form action={actions.signIn}>
            <Button type="submit" color="secondary" variant="bordered">
              Sign In
            </Button>
          </form>
        </NavbarItem>

        <NavbarItem>
          <form action={actions.signIn}>
            <Button type="submit" color="primary" variant="flat">
              Sign Up
            </Button>
          </form>
        </NavbarItem>
      </>
    );
  }

  return authContent;
}
```

**Note** - In the above the component checks the property session.status to see if it is still waiting to receive the cookie from the backened, and if so to not display anything. This prevents the Sign In and Sign Up buttons from being displayed if an authenticated user refreshes the page (causing the component to re-render and call useSession again).

Example - Header compoent with HeaderAuth

```typescript
import Link from 'next/link';
import {
  Navbar,
  NavbarBrand,
  NavbarContent,
  NavbarItem,
  Input,
} from '@nextui-org/react';
import HeaderAuth from '@/components/header-auth';

export default function Header() {
  return (
    <Navbar className="shadow mb-6">
      <NavbarBrand>
        <Link href="/" className="font-bold">
          Discuss
        </Link>
      </NavbarBrand>
      <NavbarContent justify="center">
        <NavbarItem>
          <Input />
        </NavbarItem>
      </NavbarContent>

      <NavbarContent justify="end">
        <HeaderAuth />
      </NavbarContent>
    </Navbar>
  );
}
```
