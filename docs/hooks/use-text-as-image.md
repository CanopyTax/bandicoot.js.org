# useTextAsImage

The `useTextAsImage` hook allows you to insert *unmodifiable text* into your rich text editor. This functionality
is sometimes also called "atomic elements" or "atomic blocks".

## Basic Example
```jsx
import {useTextAsImage} from 'bandicoot'

function AtomicElement() {
  const {insertTextAsImage} = useTextAsImage()

  return (
    <button onClick={() => insertTextAsImage('some unmodifiable text')}>
      Insert atomic element
    </button>
  )
}
```

## Explanation

The way that `useTextAsImage` accomplishes this is by converting a string of text into an image and then inserting
the image into the rich text editor. Since an image is treated by the browser as one big unmodifiable thing,
the cursor, [selection](https://developer.mozilla.org/en-US/docs/Web/API/Selection), and keyboard shortcuts are all
handled as if the text is unmodifiable.

Bandicoot uses [canvas.toDataURL()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toDataURL) to
create an img url for a string of text.

## Serialization and Deserialization
Although displaying text as an image has advantages for in-browser editing, it is not desirable to store things that way
in a database. So instead of storing `<img data-text-as-image="the text" src="data:garbled-weird-url">` in the database,
`useTextAsImage` converts the `<img>` into `<span data-text-as-image="the text" />` before it is saved. This process is called
serialization.

When processing stored html from a database, `useTextAsImage` will perform the reverse operation (deserialization) and will convert
the `<span>` back into an `<img>`.

## Advanced Example
```jsx
import {useState} from 'react'
import {useTextAsImage} from 'bandicoot'

function AtomicElement() {
  const [unmodifiableText, setUnmodifiableText] = useState('')
  const {insertTextAsImage} = useTextAsImage({processSerializedElement})

  return (
    <>
      <input value={unmodifiableText} onChange={setUnmodifiableText} placeholder="Unmodifiable text" />
      <button onClick={() => insertTextAsImage('some unmodifiable text')}>
        Insert atomic element
      </button>
    </>
  )

  function processSerializedElement(element, text) {
    // element is a dom element that is about to be turned into an html string that will be "saved". We can
    // customize what html is saved by modifying the element.
    element.textContent = text
  }
}
```

## Limitations
Since `useTextAsImage` creates an image for the text, it must choose the font size, family, and style upfront. If the surrounding
text changes its font in any way, the image will not change.

## Alternatives
The [useContentEditableFalse](/use-content-editable-false/README.md) hook provides an alternative way of doing the same thing.
It also can handle inserting arbitrary html instead of just text strings.

## API
```js
const {insertTextAsImage} = useTextAsImage({processSerializedElement, fontFamily})
```

### Arguments
`useTextAsImage` optionally accepts one argument - an options object with the following properties:
- `processSerializedElement`: A function that will be called with each text-as-image dom element in the rich text editor. This can be used
  to set up click listeners, change the style, etc. The function will be called for the initialHTML, calls to setHTML, and also for
  every text-as-image element that is inserted via `useTextAsImage()`. Defaults to a no-op function.
- `fontFamily` (optional): A string declaring the desired font for the text

### Return value
`useTextAsImage` returns an object with the following properties:
- `insertTextAsImage(text)`: A function that will insert unmodifiable text into the rich text editor. The function must be called
  with a string `text`.

## Notes
- `useTextAsImage` uses [`useDocumentExecCommand`](/hooks/use-document-exec-command.md) underneath the hood to call `document.execCommand()`.
