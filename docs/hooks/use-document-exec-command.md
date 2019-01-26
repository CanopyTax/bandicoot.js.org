# useDocumentExecCommand
It turns out that browsers support many rich text editing basics with a one-liner: [`document.execCommand(commandName)`](https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand).
The `useDocumentExecCommand` hook provides the logic for doing so in a way that you don't have to worry about managing the [selection](/concepts/selection.md).

To see a list of rich text features you can implement with this, check out [this MDN documentation](https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand#Commands).

## Example

```jsx
function Bold() {
  const {performCommand} = useDocumentExecCommand('bold') // Or 'italic', 'underline', or many other commands.

  return (
    <button onClick={performCommand}>
      Bold
    </button>
  )
}

function FontFamily() {
  const {performCommandWithValue} = useDocumentExecCommand('fontName')

  return (
    <button onClick={toggleFont}>
      Change font
    </button>
  )

  function toggleFont() {
    performCommandWithValue('Arial') // Changes the font family for the currently selected text to Arial
  }
}
```

## API
```js
const {performCommand, performCommandWithValue} = useDocumentExecCommand(commandName)
```

### Arguments
- `commandName` (required): The [document command](https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand#Commands) that you want to call.

### Return value
An object with the following properties:
- `performCommand()`: a function that calls document.execCommand for you. You should call this when you want to change the selected text.
- `performCommandWithValue(value)`: a function that calls document.execCommand with a [value](https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand#Parameters).
