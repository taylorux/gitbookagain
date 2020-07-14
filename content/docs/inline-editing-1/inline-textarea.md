---
title: Inline Textarea Field
prev: /docs/inline-editing/inline-text
next: /docs/inline-editing/inline-wysiwyg
consumes:
  - file: /packages/react-tinacms-inline/src/inline-field-textarea.tsx
    description: Shows InlineTextareaField
  - file: /packages/react-tinacms-inline/src/inline-field.tsx
    description: Depends on InlineField
---

# inline-textarea

The `InlineTextareaField` component represents a **multi-line text input**. It should be used for content values that are long strings: for example, a page description.

## Definition

Below is an example of how `InlineTextareaField` may be used in an [Inline Form](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/inline-editing/README.md).

```jsx
import { useLocalForm } from 'tinacms'
import { InlineForm, InlineTextareaField } from 'react-tinacms-inline'

// Example 'Page' Component
export function Page(props) {
  const [, form] = useLocalForm(props.data)
  return (
    <InlineForm form={form}>
      <h3>
        <InlineTextareaField name="title" />
      </h3>
    </InlineForm>
  )
}
```

> **Note**: Some Inline Field [styles can be extended](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/inline-editing/README.md#extending-inline-field-styles) or overridden via _Styled Components_.

## Options

| Key | Description |
| :--- | :--- |
| `name` | The path to some value in the data being edited. |

## Interface

```typescript
export interface InlineTextareaFieldProps {
  name: string
}
```

