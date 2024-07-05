# Initial Design Process

Sketching out the design of the application before doing any substantial coding is always a good idea and it will reduce the number of difficult errors to be debugged later.
The following is a list of recommended steps to help with the initial design of a Next.js application.

1. Identify all the routes and the data that each will display.
2. Create a file with path helper functions.
3. Based on step 1 create all the route folders and page.tsx files.
4. Identify all the places where data might change in the application.
5. Based on step 4 create server action stubs.
6. Identify and comment which paths will require revalidation for each server action, and then decide on the best revalidation strategy to use (time-based or on-demand).
