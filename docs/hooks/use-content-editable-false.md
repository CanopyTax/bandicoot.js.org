# useContentEditableFalse

The `useContentEditableFalse` hook allows you to insert text or html that will **not be modifiable by the user**.
In rich text editing, this functionality is sometimes also called "atomic elements" or "atomic blocks".

To accomplish this, `useContentEditableFalse` creates a span inside of the rich text editor: `<span contenteditable="false">`.
This indicates to the browser that the span and everything inside of it should be treated as a single element that cannot be modified.
It also means that the browser will delete the entire span and its contents at once with a single Backspace or Delete keypress.

## Basic Example
```jsx
import {useContentEditableFalse} from 'bandicoot'

function AtomicElement() {
  return (
    const {insertContentEditableFalseElement} = useContentEditableFalse()

    return (
      <button onClick={insertBandicoot}>
        Insert a bandicoot
      </button>
    )
  )

  function bandicoot() {
    insertContentEditableFalseElement(`Here's Bandicoot that cannot be edited`)
  }
}
```

## Limitations
`useContentEditableFalse` works in all of bandicoot's supported browsers, but there are nuances to how the browsers handle
inserting new lines right before and right after the content-editable-false element. For example, you can sometimes get
into a situation where the browser will insert two lines instead of one when you press the Enter key. Or that you can't get your
cursor to be at the beginning of a line of content-editable-false text because the browser thinks it is not editable at all.

Bandicoot attempts to make the experience consistent across browsers and natural for the user, but is not perfect at it. For example,
bandicoot inserts an empty `<span />` right before and right after the content-editable-false element to try to ensure that the browser
always allows the user to insert normal rich text content around the content-editable-false element. However, this does not solve all cases
when new lines are added and the content-editable-false-element is separated from its sibling spans.

## Alternatives
If what you need is to insert some unmodifiable **text** (instead of arbitrary html), try out the [useTextAsImage hook](/use-text-as-image/README.md),
which solves some of the limitations of `useContentEditableFalse` (while introducing a few limitations of its own).

## Advanced Example
```jsx
import {useState} from 'react'
import {useContentEditableFalse} from 'bandicoot'

function AtomicElement() {
  const [unmodifiableText, setUnmodifiableText] = useState('')
  const {insertContentEditableFalseElement} = useContentEditableFalse({processContentEditableElement})

  return (
    <>
      <input value={unmodifiableText} onChange={() => setUnmodifiableText(evt.target.value)} placeholder="Text to insert" />
      <button onClick={() => insertContentEditableFalseElement(unmodifiableText)}>
        Insert text
      </button>
    </>
  )

  function processContentEditableElement(element) {
    // The element is a dom element that is unmodifiable
    element.style.fontWeight = 'bold'
    element.style.color = 'blue'
  }
}
```

## API
```
const {insertContentEditableFalseElement} = useContentEditableFalse({processContentEditableElement})
```

### Arguments
`useContentEditableFalse` can be called with no arguments or with an object with the following properties:
- `processContentEditableElement`: A function that will be called with each unmodifiable dom element in the rich text editor. This can be used
  to set up click listeners, change the style, etc. The function will be called for unmodifiable elements in the initialHTML, calls to setHTML, and also for
  every unmodifiable element that is inserted via `useContentEditableFalse()`. Defaults to a no-op function.

### Return value
`useContentEditableFalse` returns an object with the following properties:
- `insertContentEditableFalseElement`: A function that will insert unmodifiable html into the rich text editor. This function must be called
  with one argument, a string of html.

## Notes
- `useContentEditableFalse` uses [`useDocumentExecCommand`](/hooks/use-document-exec-command.md) underneath the hood to call `document.execCommand()`.
