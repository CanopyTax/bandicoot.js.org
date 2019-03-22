# Undo / Redo
> asdf...as...asdf
>
> &#8212; Anonymous

## Definition

Undo and Redo are operations a user performs through keyboard shortcuts or button clicks.

In bandicoot editors, a user can always undo or redo their previous actions with the keyboard shortcuts Ctrl/Cmd + Z (undo)
or Ctrl/Cmd + Y (redo).

## Example
To undo or redo the last action programmatically, use the [useDocumentExecCommand](/hooks/use-document-exec-command.md) hook:

```jsx
import {useDocumentExecCommand} from 'bandicoot'

function UndoButton() {
  const {performCommand} = useDocumentExecCommand('undo')

  return (
    <button onClick={performCommand}>
      Undo
    </button>
  )
}

function UndoButton() {
  const {performCommand} = useDocumentExecCommand('redo')

  return (
    <button onClick={performCommand}>
      Redo
    </button>
  )
}
```

[Read MDN docs for more detail](https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand#Commands)

## Caviats / Notes
If you want your dom actions to be "undoable" and "redoable", you must make your modifications with
[document.execCommand](/hooks/use-document-exec-command.md), [selection](/concepts/selection.md), and [range](/concepts/range.md).

For example, to delete dom nodes and text in an "undoable" way, you must highlight the text programatically and then delete it
with execCommand. Doing `element.remove()` will not result in the operation being "undoable". Additionally, modifying a selection
and range programatically instead of using `document.execCommand` to modify it will result in the operation not being "undoable."

```js
function undoableDelete(element) {
  // The newRange will highlight the entire element
  const newRange = document.createRange()
  newRange.selectNode(element)

  // Make that range the active one
  const selection = window.getSelection()
  selection.removeAllRanges()
  selection.addRange(newRange)

  // Now delete the the highlighted text with execCommand
  document.execCommand('delete')

  // Now undo/redo will include this operation!
}
```
