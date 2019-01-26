# Content editable

> I would hardly call that content.
>
> &#8212; Anonymous

## Definition

Content editable is the DOM concept that makes in-browser rich text editing possible. It makes it possible
for users to use HTML to style their input, instead of all input being treated as a raw string.

In bandicoot, the [RichTextEditor component](/components/rich-text-editor.md) is in charge of rendering
a content editable div.

You can read more about content editable [in these MDN docs](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/contentEditable).

## Example

To get started with content editable divs, simply add the `contenteditable` attribute to a dom element:

```html
<div contenteditable="true" />
```

Now your div will be modifiable by users and can contain rich text. See
[this code pen](https://codepen.io/joeldenning/pen/jdqWZO) for a simple example.

The good news is that bandicoot takes care of a lot of the content editable stuff without you
having to think about it.
