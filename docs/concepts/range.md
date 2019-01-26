# Range

> Wait I thought I only had to learn about selection.
>
> &#8212; Anonymous

## Definition
Range is a DOM concept that keeps track of the cursor position and highlighted text.

[Read more on MDN](https://developer.mozilla.org/en-US/docs/Web/API/Range)

## What this means for you
Bandicoot hides much or all of the tricky range stuff from you. For most cases, you won't have to deal with
it at all.

If you do need to deal with the DOM's range yourself, bandicoot does not provide any abstraction on top
of what the DOM already has.

## Relationship to "selection"
Selection and Range are easy to confuse because they're DOM concepts that are highly related.

The most important thing to know is the DOM selection almost always contains exactly one DOM Range,
and that the thing that is most useful to you is the Range.

You can read more about this in our [selection docs](/concepts/selection.md).

## Example
Here's a very basic example of using DOM range:

```js
const selection = window.getSelection()
if (selection.rangeCount > 0) {
  const range = selection.getRangeAt(0)
  console.log('the currently selected text is', range.toString())
}
```
