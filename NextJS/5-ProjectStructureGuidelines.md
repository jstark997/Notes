# Project Structure Guidelines

1. All pages of the app should be placed in the src/app directory and sub-directories that define the routes.
2. All other code for components, data, utilities, etc. should be placed in the src directory outside of the app directory to avoid confusion as to which directories contain pages and define routes.

```
- src/
  - app/
    - about/
      - details/
        - page.tsx
      - page.tsx
    - page.tsx
  - components/
  - data/
  - utils/
```
