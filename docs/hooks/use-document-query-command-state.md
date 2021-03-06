# useDocumentQueryCommandState
Browsers support detecting if the currently selected text is bolded, underlined, italicized, and much more with a one liner: [`document.queryCommandState(commandName)`](https://developer.mozilla.org/en-US/docs/Web/API/Document/queryCommandState).
Additionally, you can detect what the current value is (e.g., font family is "Arial") with `document.queryCommandValue(commandName)`.

The `useDocumentQueryCommandState` hook provides the logic for doing so without you having to deal with the DOM or [selection](/concepts/selection.md).

To see a list of rich text features that you can use this with, check out [this MDN documentation](https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand#Commands).

## Basic Example
```jsx
function Bold() {
  // isActive is a boolean that indicates if the currently selected text is bold.
  const {isActive} = useDocumentQueryCommandState('bold')

  return (
    <button className={isActive ? 'control-button-active' : ''}>
      Bold
    </button>
  )
}

function FontFamily() {
  // activeValue is a string that indicates the currently selected fontFamily
  const {activeValue} = useDocumentQueryCommandState('fontFamily')

  return (
    <div>The currently selected text is using font family {activeValue}</div>
  )
}

function Header() {
  // isActive is a boolean that indicates if the current line/selection is wrapped by h1
  const {isActive} = useDocumentQueryCommandState('formatBlock', 'h1')

  return (
    <button className={isActive ? 'control-button-active' : ''}>
      H1
    </button>
  )
}
```

## Advanced Example
If you wish to customize when `isActive` is true, you can pass in a second argument to the hook. The second argument is
a string value that will be compared to the return value of `document.queryCommandValue(commandName)`.

```jsx
function IsH1() {
  const {isActive} = useDocumentQueryCommandState('formatBlock', 'h1');

  return (
    <div className={isActive ? 'green' : ''}>This text is green when the selected text is an H1 element</div>
  )
}
```

## API
```js
const {isActive, activeValue} = useDocumentQueryCommandState(commandName, activeValueMatch)
```

### Arguments
- `commandName` (required): The [document command](https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand#Commands) that you want to check the status of.
- `activeValueMatch` (optional): A string that allows you to customize when `isActive` is true or false. When omitted, `isActive` is simply the return value of
`document.queryCommandState(commandName)`, but when `activeValueMatch` is provided, `isActive` will be true when `document.queryCommandValue(commandName)` equals
the provided `activeValueMatch`.

### Return value
An object with the following properties:
- `isActive`: A boolean that indicates whether the currently selected text is active for the specified "command". This is done with [`document.queryCommandState(commandName)`](https://developer.mozilla.org/en-US/docs/Web/API/Document/queryCommandState).
- `activeValue`: A string that tells you what the active value is for the specified "command". For example, this can tell you the font family of the currently selected text.
