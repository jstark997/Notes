# Button Loading Spinner

The Button component from 'nextui-org/react' has a property isLoading that if true will cause the Button component to display a loading spinner.

Example:

```typescript
'use client';

import { useFormStatus } from 'react-dom';
import { Button } from '@nextui-org/react';

interface FormButtonProps {
  children: React.ReactNode;
}

export default function FormButton({ children }: FormButtonProps) {
  const { pending } = useFormStatus();

  return (
    <Button type="submit" isLoading={pending}>
      {children}
    </Button>
  );
}
```
