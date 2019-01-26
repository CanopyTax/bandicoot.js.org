# Selection

> I wish they would have called it "impossible-to-understand-cursor-information."
>
> &#8212; Anonymous

## Definition

Selection is the DOM concept for keeping track of the cursor position and highlighted text.

[Read more on MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection)

Selection is notoriously tricky to understand and can be quite painful to work with. Reading from and
manipulating the DOM's selection is required for rich text editing.

## What this means for you
Bandicoot hides much or all of the tricky selection stuff from you. For most cases, you won't have to deal with
it at all.

If you do need to deal with the DOM's selection yourself, bandicoot does not provide any abstraction on top
of what the DOM already has.

## Example
Here's a very basic example of using DOM selection:

```js
const selection = window.getSelection()
if (selection.rangeCount > 0) {
  const range = selection.getRangeAt(0)
  console.log('the currently selected text is', range.toString())
}
```

## What about "range"?
When learning about selection, you'll quickly discover that what you really need is a
[Range](https://developer.mozilla.org/en-US/docs/Web/API/Range) object instead of a
[Selection](https://developer.mozilla.org/en-US/docs/Web/API/Selection) object.

Check out our [documentation for Range](/concepts/range.md) for more information.
