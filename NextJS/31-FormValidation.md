# Form Validation

Form validation is typically done using the useFormState hook which is passed a server action that performs the validation. This is so that any errors can be returned to the browser and displayed to the user.

**Note** - Hooks, such as useFormState, can only be used in client components.

## FormState Type Errors for useFormStae Hook

Watch out for type errors when using useFormState invovling the second argument which is the initial state of the FormState object.

Example - Component:

```typescript
'use client'

import { useFormState } from 'react-dom';

funciton Component() {
  const [formState, action] = useFormState(serverAction, _1_);

  return <form action={action}></form>
}
```

Example - Server action

```typescript
'use server';

async function serverAction(formState: _2_, formData: FormData) {
  return _3_;
}
```

In the above component and server action examples 1, 2, and 3 must all be the same type. Otherwise an error will be thrown.

## Complete Validation Example

In the following example for a create topic form, an interface is created (in the server action file) to define the type of the FormState object in order to avoid any type errors thrown by the useFormState hook. The defined type has the structure of the result returned by the validator (after it has been flattened). Since there are no validation errors initially the initial state of the useFormState hook is an empty errors object.

Server Action:

```typescript
'use server';

import { z } from 'zod';

const createTopicSchema = z.object({
  name: z
    .string()
    .min(3)
    .regex(/[a-z-]/, {
      message: 'Must be lowercase letters or dashes without spaces',
    }),
  description: z.string().min(10),
});

interface CreateTopicFormState {
  errors: {
    name?: string[];
    description?: string[];
  };
}

export async function createTopic(
  formState: CreateTopicFormState,
  formData: FormData
): Promise<CreateTopicFormState> {
  const result = createTopicSchema.safeParse({
    name: formData.get('name'),
    description: formData.get('description'),
  });

  if (!result.success) {
    return {
      errors: result.error.flatten().fieldErrors,
    };
  }

  return {
    errors: {},
  };

  // TODO: revalidate the homepage
}
```

Component:

```typescript
'use client';

import { useFormState } from 'react-dom';
import {
  Input,
  Button,
  Textarea,
  Popover,
  PopoverTrigger,
  PopoverContent,
} from '@nextui-org/react';
import * as actions from '@/actions';

export default function TopicCreateForm() {
  const [formState, action] = useFormState(actions.createTopic, {
    errors: {},
  });

  return (
    <Popover placement="left">
      <PopoverTrigger>
        <Button color="primary">Create a Topic</Button>
      </PopoverTrigger>
      <PopoverContent>
        <form action={action}>
          <div className="flex flex-col gap-4 p-4 w-80">
            <h3 className="text-lg">Create a Topic</h3>
            <Input
              name="name"
              label="Name"
              labelPlacement="outside"
              placeholder="Name"
            />
            <Textarea
              name="description"
              label="Description"
              labelPlacement="outside"
              placeholder="Describe your topic"
            />
            <Button type="submit">Submit</Button>
          </div>
        </form>
      </PopoverContent>
    </Popover>
  );
}
```
