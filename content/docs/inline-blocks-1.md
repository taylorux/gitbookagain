---
title: Inline Blocks
prev: /docs/inline-editing
next: /docs/inline-blocks/block-text
consumes:
  - file: /packages/react-tinacms-inline/src/inline-form.tsx
    description: InlineForm & useInlineForm
  - file: /packages/react-tinacms-inline/src/blocks/block.ts
    description: Shows Block interface
  - file: /packages/@tinacms/fields/src/plugins/BlocksFieldPlugin.tsx
    description: Shows Block Template interface
  - file: /packages/react-tinacms-inline/src/blocks/inline-field-blocks.tsx
    description: Shows Inline Blocks interface and gives example
  - file: /packages/react-modals/src/ModalProvider.tsx
    description: Uses ModalProvider for Block Settings
---

# inline-blocks

Inline Blocks combine the content modelling flexibility of regular [Blocks](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/blog/what-are-blocks/README.md) with the improved editing experience of [Inline Editing](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/inline-editing/README.md).

> **Please note:** This is considered an unstable API. Any breaking changes will be shared in the weekly [Release Notes](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/blog/README.md). That said, we will do our best to keep this document up-to-date.

![TinaCMS: Inline Blocks Gif](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/gif/inline-blocks.gif)

## Creating a Block

A _block_ is **made of two parts**: _a component_ that renders in edit mode, and _a template_ to configure fields, defaults and other required data.

```typescript
interface Block {
  Component: React.FC<BlockComponentProps>
  template: BlockTemplate
}
```

### Part 1: Block Component

```jsx
import { BlocksControls, BlockTextArea } from 'react-tinacms-inline'

// Example 'Heading' Block
export function Heading(props) {
  return (
    <BlocksControls index={props.index}>
      <BlockTextarea name="text" />
    </BlocksControls>
  )
}
```

The _Block Component_ is passed `index`, its position in the block order, and `data`, the source data.

```typescript
interface BlockComponentProps {
  index: number
  data: any
}
```

Since it **renders in 'edit mode,'** this component should display `BlocksControls` and at least one _Block Field_. `BlocksControls` is the UI for editing, deleting, or moving blocks. A _Block Field_ is, at its most basic, an inline input field stripped of styling to blend in with the site.

![TinaCMS: Inline Block Controls](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/img/inline-blocks/block-controls.png)

The image above shows the _Textarea Block_ field in use.

**Available Block Fields**:

* [Text Block](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/inline-blocks/block-text/README.md)
* [Textarea Block](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/inline-blocks/block-textarea/README.md)
* [Image Block](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/inline-blocks/block-image/README.md)

### Part 2: Block Template

```jsx
export const heading_template = {
  type: 'heading',
  label: 'Heading',
  defaultItem: {
    _template: 'heading',
    text: 'At vero eos et accusamus',
  },
  key: 'heading-block',
  fields: [],
}
```

The _Inline Block Template_ **configures the block** with the CMS. It has a similar shape to a Regular [Block Template](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/fields/blocks/README.md#block-template-options) definition.

| Key | Type | Purpose |  |
| :--- | :---: | ---: | :--- |
| `type` | `string` | This value connects source block data with the template. |  |
| `label` | `string` | A human readable label. |  |
| `defaultItem?` | \`object | \(\(\) =&gt; object\)\` | Populates new blocks with default data. |
| `key` | `string` | A unique value to optimize the [rendering of the list](https://reactjs.org/docs/lists-and-keys.html) |  |
| `fields` | `Array` | Populates fields in the [Settings Modal](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/content/docs/link%20to%20settings%20modal/README.md). |  |

## Configuring Inline Blocks with Inline Form

The initial steps to configuring _Inline Blocks_ involve setting up an [_Inline Form_](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/inline-editing/README.md#inlineform-and-inlinefield) on the page or component where the blocks should render. Then, you should [add controls](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/inline-editing/README.md#inline-form-controls) to handle editing state. Finally, you can use a component called `InlineBlocks` that **renders blocks in order** based on the source data.

### The Steps:

1. Wrap your component with `InlineForm`, pass the `form` object.
2. Set up [Inline Controls](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/inline-editing/README.md#inline-form-controls).
3. Configure `InlineBlocks`, pass the `name` and `blocks` values.

```jsx
import { useJsonForm } from 'next-tinacms-json'
import { InlineForm, InlineBlocks } from 'react-tinacms-inline'
import {
  BodyCopy,
  body_copy_template,
  Heading,
  heading_template,
  Image,
  image_template,
} from './blocks'
import { EditToggle, DiscardButton, SaveButton } from './inline-ui'

/*
 ** Example 'PageBlocks' Component
 */

export default function PageBlocks({ jsonFile }) {
  // Creates the form
  const [, form] = useJsonForm(jsonFile)

  return (
    <InlineForm form={form}>
      <EditToggle />
      <SaveButton />
      <DiscardButton />
      <InlineBlocks name="blocks" blocks={PAGE_BLOCKS} />
    </InlineForm>
  )
}

/*
 ** Multiple blocks are grouped into a single object,
 ** with their associated Component and template values.
 ** This object is passed to InlineBlocks
 */

const PAGE_BLOCKS = {
  heading: {
    Component: Heading,
    template: heading_template,
  },
  body_copy: {
    Component: BodyCopy,
    template: body_copy_template,
  },
  image: {
    Component: Image,
    template: image_template,
  },
}
```

### _InlineBlocks_ Interface

To be configured properly, `InlineBlocks` requires `name` and `blocks`.

```typescript
interface InlineBlocksProps {
  name: string
  blocks: {
    [key: string]: Block
  }
}
```

| Key | Type | Purpose |
| :--- | :---: | ---: |
| `name` | `string` | The path to the **source data** for the blocks. |
| `blocks` | `object` | An object composed of individual [Blocks](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/inline-editing/inline-blocks/README.md#creating-a-block). |

### Blocks Source Data

The source data for the blocks in the example above could look something like this:

```javascript
// Example blocks JSON source file
{
  "blocks": [
    {
      "_template": "image",
      "src": "/img/bali-1.jpg",
      "alt": "bali-viaje"
    },
    {
      "_template": "heading",
      "text": "Ne quaesiveris extra."
    },
    {
      "_template": "body_copy",
      "text": "Sed ut perspiciatis unde omnis iste natus error."
    }
  ]
}
```

The key \("blocks" in this example\) for the array of individual blocks must match the `name` value passed to `InlineBlocks`.

Each individual _block_ object must have a `_template` value to connect its data with a block template. This value should match the `type` value defined in the associated block template.

## Using the Settings Modal

There may be times when a _Block_ **needs more field inputs** than a single inline field. For example, an image may need a field for the ‘alt’ tag. For this metadata, you can use `ModalProvider` and add additional fields to the _Block Template_.

![TinaCMS: Inline Blocks Settings Modal](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/img/inline-blocks/settings-modal.png)

### 1. Define Fields in the Block Template

Field definitions added to the `fields` property will render in the _Settings Modal_. These fields are defined exactly the same as regular [sidebar fields](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/fields/README.md).

```javascript
const image_template = {
  type: 'image',
  label: 'Image',
  defaultItem: {
    _template: 'image',
    src: '/img/bali-1.jpg',
    alt: '',
  },
  key: 'image-block',
  /*
   ** Define fields to render
   ** in a Settings Modal form
   */
  fields: [
    {
      name: 'alt',
      label: 'Alt Text',
      component: 'text',
    },
  ],
}
```

### 2. Use the field data in your Block Component

You can then use that field data in the Block Component. In this example, the data will populate the `alt` attribute for the image.

```jsx
export function Image({ data, index }) {
  return (
    <>
      <div>
        <BlocksControls index={index}>
          <BlockImage
            name="src"
            parse={filename => `/img/${filename}`}
            uploadDir={() => '/public/img/'}
          >
            {/*
             ** The 'alt' data from the
             ** 'settings' is consumed
             */}
            <img src={data.src} alt={data.alt} />
          </BlockImage>
        </BlocksControls>
      </div>
    </>
  )
}
```

### 3. Add the `ModalProvider`

To use the `ModalProvider`, wrap it around the `InlineForm` that renders `InlineBlocks`. This will provide the _Settings Modal_ view. The fields defined in the block template will **populate the form** in that modal.

```jsx
import { useJsonForm } from 'next-tinacms-json'
import { ModalProvider } from 'tinacms'
import { InlineForm, InlineBlocks } from 'react-tinacms-inline'

export default function IndexBlocks({ jsonFile }) {
  const [, form] = useJsonForm(jsonFile)

  // Wrap InlineForm with the ModalProvider
  return (
    <ModalProvider>
      <InlineForm form={form}>
        {/*
         ** //...
         */}
      </InlineForm>
    </ModalProvider>
  )
}
```

