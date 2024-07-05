# Redirect

The **redirect** from the 'next/navigation' module signals to Next.js to redirect to the specified path by throwning a special error called 'NEXT_REDIRECT'. Next.js interprets that error as a command to redirect.

**Important** - However, if the call to the redirect function is within a try/catch block, the special 'NEXT_REDIRECT' error will be caught and not handled by Next.js normally. So it is important to be sure not to place calls to redirect within a try/catch block.
