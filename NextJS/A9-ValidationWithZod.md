# Validation With Zod

Zod is a library that helps to automate user input validation.

## Installation

```sh
npm install zod
```

## Schema

Using the Zod library a schema object is created which implements the validation rules for a specific type of input.

Example:

```typescript
import { z } from 'zod';

const createTopicSchema = z.object({
  named: z
    .string()
    .min(3)
    .regex(/^[a-z-]+$/, {
      message: 'Must be lowercase letters or dashes without spaces',
    }),
  description: z.string().min(10),
});
```

The above Zod schema will check that an object has a name property that is a string with at least 3 characters that match the regex pattern specified, and a description property that is a string with at least 10 characters.

## Validation

Once a schema has been created it can be used to validate data by calling the safeParse method on the data to validate.
The safeParse method will return an object with a success property indicating whether the data is valid and a data property with the data that was passed:

Example - Valid data:

```typescript
{
  success: true,
  data: {
    name: 'javascript',
    description: 'a place to talk about JS
  }
}
```

Example - Invalid data:

```typescript
{
  success: false,
  error: {
    issues: [
      /* object about a failed validation rule */.
      /* object about a failed validation rule */.
      /* object about a failed validation rule */
    ]
  }
}
```

Example - Code for schema creation and use

```typescript
import { z } from 'zod';

const createTopicSchema = z.object({
  named: z
    .string()
    .min(3)
    .regex(/^[a-z-]+$/, {
      message: 'Must be lowercase letters or dashes without spaces',
    }),
  description: z.string().min(10),
});

export async function createTopic(formData: FormData) {
  const result = createTopicSchema.safeParse({
    name: formData.get('name'),
    description: formData.get('description'),
  });

  if (!result.sucess) {
    console.log(result.error.flatten().fieldErrors);
  }

  //Rest of cade ...
}
```

**Note** - The objects in the error array returned by Zod can be complex. To simply can flatten the array and reference fieldErrors to just get the errors for each piece of data that failed validation.
