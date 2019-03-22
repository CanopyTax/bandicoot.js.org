# useFormatBlock

Use this hook to add an HTML block-level element around the line(s) containing the current selection, replacing the block element containing the line if one exists. Under the hood, bandicoot employs [useDocumentExecCommand](/hooks/use-document-exec-command.md).

If the provided value is already present, useFormatBlock will switch it out for a `<div>`

## Example

```jsx
function Bold() {
  const {formatBlock} = useFormatBlock()

  return (
    <button onClick={formatBlock('H1')}>
      H1
    </button>
  )
}
```

## API
```js
const {formatBlock} = useFormatBlock()
```

### Arguments
- none

### Return value
An object with the following properties:
- `formatBlock(value)`: a function that calls document.execCommand('formatBlock', value) for you. You should call this when you want to add a block-level element around the current selection.