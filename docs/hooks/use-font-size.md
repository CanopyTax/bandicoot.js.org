# useFontSize

Font size can be changed with a one liner DOM command: [`document.execCommand('fontSize', null, 1)`](https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand#Commands).
Unfortunately, though, browsers only support setting [an integer font size](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/font#Attributes)
instead of letting you specify pixels, ems, or rems.

To work around this, bandicoot provides the `useFontSize` hook, which allows you to specify a change of font size to
[any valid css font-size](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size).

## Example
```jsx
import {useFontSize} from 'bandicoot'

function FontSize() {
  const {currentlySelectedFontSize, setSize} = useFontSize({defaultFontSize: '16px', fontSizes})

  return (
    <>
      {changeSizeButton('10px', '10')}
      {changeSizeButton('16px', '16')}
      {changeSizeButton('24px', '24')}
      Currently selected size: {currentlySelectedFontSize}
    </>
  )

  function changeSizeButton(size, label) {
    return (
      <button onClick={() => setSize(size)}>
        {label}
      </button>
    )
  }
}

const fontSizes = [
  '10px',
  '16px',
  '24px',
]
```

## API
```js
const {currentlySelectedFontSize, setSize} = useFontSize({defaultFontSize, fontSizes})
```

### Arguments
useFontSize requires an object to be passed as an argument, with the following properties:
- `fontSizes` (required): An array of string font sizes. The strings must be
  a [valid font-size css value](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size).
- `defaultFontSize` (required): A string font size that will be returned as the currentlySelectedFontSize when
  the rich text editor is not focused or selected.

### Return value
useFontSize returns an object with the following properties:
- `currentlySelectedFontSize`: The string font size that is currently selected.
- `setSize(fontSize)`: A function that requires on argument, `size`. When called, the `setSize` function will change
  the currently selected text to be the new font size.

## Notes
- `useFontSize` uses [`useDocumentExecCommand`](/hooks/use-document-exec-command.md) underneath the hood to call `document.execCommand()`.
