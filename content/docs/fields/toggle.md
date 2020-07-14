---
title: Toggle Field
prev: /docs/fields/color
next: /docs/fields/select
consumes:
  - file: /packages/tinacms/src/plugins/fields/ToggleFieldPlugin.tsx
    details: Shows toggle field interface and config options
  - file: /packages/@tinacms/fields/src/Toggle.ts
    details: Shows toggle field interface and config options
---

# toggle

The `toggle` field represents a true/false toggle. This field is typically used for boolean content values. You could use this to toggle a certain feature on the page on or off.

![tinacms-toggle-field](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/img/fields/toggle.png)

## Definition

Below is an example of how a `toggle` field could be defined in a Gatsby remark form. [Read more on passing in form field options](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/gatsby/markdown/README.md#customizing-remark-forms).

```javascript
const BlogPostForm = {
  fields: [
    {
      name: 'rawFrontmatter.is_hidden',
      component: 'toggle',
      label: 'Hide Details',
      description: 'Choose whether to hide or show details here',
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

## Interface

```typescript
interface ToggleConfig {
  component: 'Toggle'
  name: string
  label?: string
  description?: string
}
```

