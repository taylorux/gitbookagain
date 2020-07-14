---
title: Inline Image Field
prev: /docs/inline-editing/inline-wysiwyg
next: /docs/inline-blocks
consumes:
  - file: /packages/react-tinacms-inline/src/inline-field-image.tsx
    description: Shows InlineImageField
  - file: /packages/react-tinacms-inline/src/inline-field.tsx
    description: Depends on InlineField
---

# inline-image

The `InlineImageField` field represents an image input. This field supports drag and drop upload, or via clicking on the image to select media from the local filesystem.

## Definition

Below is a simple example of how `InlineImageField` could be implemented in in an [Inline Form](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/inline-editing/README.md).

```jsx
import ReactMarkdown from 'react-markdown'
import { useLocalForm } from 'tinacms'
import { InlineForm, InlineImagefield } from 'react-tinacms-inline'

// Example 'Page' Component
export function Page(props) {
  const [data, form] = useLocalForm(props.data)
  return (
    <InlineForm form={form}>
      <InlineImageField
        name="hero_image"
        parse={filename => `/images/${filename}`}
        uploadDir={() => '/public/images/'}
      />
    </InlineForm>
  )
}
```

There are two ways to use `InlineImageField`, with and without children. If no children are passed, `InlineImageField` will render a default `img` element. However, you may want more control over the image behavior, in which case you can [**pass children**](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/inline-editing/inline-image/README.md#example) to `InlineImageField`.

## Options

| Key | Description |
| :--- | :--- |
| `name` | The path to some value in the data being edited. |
| `parse` | Defines how the actual front matter or data value gets populated. The name of the file gets passed as an argument, and one can set the path this image as defined by the uploadDir property. |
| `uploadDir` | Defines the upload directory for the image. All of the post data is passed in, `fileRelativePath` is most useful in defining the upload directory, but you can also statically define the upload directory. |
| `children` | Any child elements. |

## Interface

```typescript
export interface InlineImageFieldProps {
  name: string
  parse(filename: string): string
  uploadDir(form: Form): string
  children?: any
}
```

## Example

Below is an example of how you could **pass children** to `InlineImageField` to work with _Gatsby Image_. Read more on [proper image paths](https://github.com/taylorux/tinacms.org/tree/ec3e5c1e5736454379815f45595441bd79d85a2d/docs/fields/image/README.md#proper-image-paths-in-gatsby) in Gatsby to get context on the `parse` & `uploadDir` configuration.

```jsx
import {
  Inlineform,
  InlineImageField,
  InlineTextareaField,
} from 'react-tinacms-inline'
import { useLocalRemarkForm } from 'gatsby-tinacms-remark'
import Img from 'gatsby-image'

// Using InlineImageField with Gatsby Image
export function Hero({ data }) {
  const [post, form] = useLocalRemarkForm(data.markdownRemark)

  return (
    <InlineForm form={form}>
      <InlineImageField
        name="rawFrontmatter.thumbnail"
        parse={filename => (filename ? `./${filename}` : null)}
        uploadDir={blogPost => {
          const postPathParts = blogPost.initialValues.fileRelativePath.split(
            '/'
          )
          const postDirectory = postPathParts
            .splice(0, postPathParts.length - 1)
            .join('/')

          return postDirectory
        }}
      >
        <Img
          fluid={post.frontmatter.thumbnail.childImageSharp.fluid}
          alt={post.frontmatter.thumbnail.altText}
        />
      </InlineImageField>
      <h1>
        <InlineTextareaField name="rawFrontmatter.title" />
      </h1>
    </InlineForm>
  )
}
```

