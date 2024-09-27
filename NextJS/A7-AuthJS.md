# AuthJS

This is popular library the helps to implement authorization, also known as 'next-auth'.

For this example GitHub OAuth will be used to provide authentication.

## GitHub OAuth

The following assumes you have a GitHub account.

1. Go to 'github.com/settings/applications/new' for your account.
2. Create an OAuth app.
   a. Add the application name.
   b. Enter the application's homepage URL (for a dev app can be http://localhost:3000).
   c. Enter the authorization callback URL (for example http://localhost:3000/api/auth/callback/github).
   d. Click on the 'Register application' button.
3. GitHub will navigate to the application's GitHub OAuth page, where you should see the client_id.
4. Generate a client secret by clicking on the 'Generate a new client secret' button.
5. Securely make note of or copy the client secret as it will not be displayed again once you navigate away from the page.

## Environment Variable File

1. In the root project directory create a file called '.env.local'.
2. Add the following values to the environment variable file:

```
GITHUB_CLIENT_ID=<add client_id value for GitHub OAuth app>
GITHUB_CLIENT_SECRET=<add the client secret created for the GitHub OAuth app>
AUTH_SECRET=<add any random series of numbers and letters>
```

## Install AuthJS libraries

Need to install 3 libraries:

- auth/core
- auth/prisma-adapter (required if using Prisma)
- next-auth

```sh
npm install --save-exact @auth/core@0.18.1 @auth/prisma-adapter@1.0.6 next-auth@5.0.0-beta.3
```

The above versions will work with version 14.0 of Next.js.

## Configure NextAuth Object

In the src directory create a file called 'auth.ts' with the following content:

```typescript
import NextAuth from 'next-auth';
import Github from 'next-auth/providers/github';
import { PrismaAdapter } from '@auth/prisma-adapter';
import { db } from '@/db';

const GITHUB_CLIENT_ID = process.env.GITHUB_CLIENT_ID;
const GITHUB_CLIENT_SECRET = process.env.GITHUB_CLIENT_SECRET;

if (!GITHUB_CLIENT_ID || !GITHUB_CLIENT_SECRET) {
  throw new Error('Missing github oauth credentials');
}

export const {
  handlers: { GET, POST },
  auth,
  signOut,
  signIn,
} = NextAuth({
  adapter: PrismaAdapter(db),
  providers: [
    Github({
      clientId: GITHUB_CLIENT_ID,
      clientSecret: GITHUB_CLIENT_SECRET,
    }),
  ],
  callbacks: {
    // Usually not needed, here we are fixing a bug in nextauth
    async session({ session, user }: any) {
      if (session && user) {
        session.user.id = user.id;
      }

      return session;
    },
  },
});
```

This file implements the authentication functionality using OAuth for the project. The PrismaAdapter is used to create a new user in the database when one authenticates for the first time. The code retrieves the environment variables for the GitHub client id and client secret and configures the NextAuth object. The NextAuth object is destructured and the requst handlers GET and POST (called by GitHub during authentication) and the functions auth, signOut and signIn are exported to make them available to any part of the application.

## Create Authentication API Request Handlers Endpoints

In the src directory create the file 'app/api/auth/[...nextauth]/route.ts'.

```
- my-app/
  - .next/
  - node_modules/
  - prisma/
  - public/
  - src/
    - db/
    - app/
      - api
        - auth/
          - [...nextauth]
            - route.ts
```

The route.ts file is a special file that Next.js uses to define API endpoints. In this file can be defined API request handlers (for HTTP methods GET and POST) that can be called to interact with the API of an external web service.

Code in route.ts to interact with GitHub OAuth:

```typescript
export { GET, POST } from '@auth';
```

The code just imports and then exports again the GET and POST request handlers from the NextAuth object that GitHub will use for authentication.

## Server Actions to Sign In and Sign Out the User (Optional)

This step is optional but highly recommended. User authentication state is a piece of data that will change over time and probably should be managed by the application. By creating server actions for signing in and out, other engineers on the project can make use of them to easily sign a user in or out.

Inside the src directory make a directory called 'actions' and then a file called 'index.ts'.

Code in index.ts:

```typescript
'use server';

import * as auth from '@/auth';

export async function signIn() {
  return auth.signIn('github');
}

export async function signOut() {
  return auth.signOut();
}
```

In the above the signIn and signOut functions from auth (the NextAuth object) are wrapped into server actions. The auth signIn function takes a string argument that identifies which provider to use to sign in with, in this case it is GitHub.
