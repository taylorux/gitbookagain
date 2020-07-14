---
title: Select Field
prev: /docs/fields/toggle
next: /docs/fields/group
consumes:
  - file: /packages/@tinacms/fields/src/Select.tsx
    details: Shows select field and Option interfaces
  - file: /packages/tinacms/src/plugins/fields/SelectFieldPlugin.tsx
    details: Shows select field and Option interfaces
---

# select

The `select` field represents a select element.

![TinaCMS Select Field](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/img/fields/select-field.png)

## Definition

Below is an example of how a `select` field could be defined in a [form](http://localhost:3000/docs/forms). Read more on passing in [form field options](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/gatsby/markdown/README.md#customizing-remark-forms)\) for Gatsby.

```javascript
const BlogPostForm = {
  fields: [
    {
      name: 'frontmatter.authors',
      component: 'select',
      label: 'Author',
      description: 'Select an author for this post',
      options: ['Arundhati Roy', 'Ruth Ozeki', 'Zadie Smith'],
    },
    // ...
  ],
}
```

## Options

* `name`: The path to some value in the data being edited.
* `component`: The name of the React component that should be used to edit this field. Available field component types are [defined here](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/fields/README.md)
* `label`: A human readable label for the field. This label displays in the sidebar and is optional. If no label is provided, the sidebar will default to the name.
* `description`: An optional description that expands on the purpose of the field or prompts a specific action.
* `options`: An array of strings.

## Interface

```typescript
interface SelectField {
  name: string
  component: 'select'
  label?: string
  description?: string
  options: string[]
}
```

