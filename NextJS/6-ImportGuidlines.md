# Import Guidelines

The path for imported, locally defined, components can be specified in 3 ways:

1. Absolute path
2. Relative path
3. Path shortcut

Relative path example:

```typescript
import Header from '../components/header';
```

In the above the Header component is imported using the relative path syntax where '..' specifies the parent directory of the directory of the component that the import statement is included in.

Shortcut example:

```typescript
import Header from '@/components/header';
```

In the above the Header component is imported using the path shortcut '@' which Next.js defines as the src directory.
