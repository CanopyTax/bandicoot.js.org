# RichTextContext

RichTextContext is [React context](https://reactjs.org/docs/context.html) that is used by bandicoot's built-in
components and hooks.

The context value is provided by [RichTextContainer](/components/rich-text-container.md) and is consumed by
[RichTextEditor](/components/rich-text-editor.md) and the [bandicoot hooks](/hooks/use-document-exec-command.md).

## A word of caution

Although RichTextContext is exported from bandicoot, you normally do not need to use it directly.
Instead, you should be able to accomplish most of what you need via [bandicoot's built-in hooks](/hooks/use-document-exec-command.md).

So before using RichTextContext directly, look for a hook that already does what you need. If you
find a use case not supported by the current hooks, consider [opening a Github issue](https://github.com/CanopyTax/bandicoot/issues/new)
where we can discuss adding a new hook to bandicoot. That way, everyone will benefit from your work.

## Example
```jsx
import {useContext, useState} from 'react'
import {RichTextContext} from 'bandicoot'

function MyControlButton() {
  const richTextContext = useContext(RichTextContext)
  const [numSelectionChanges, setNumSelectionChanges] = useState(0)

  useEffect(() => {
    richTextContext.addSelectionChangedListener(selectionChanged)
    return () => richTextContext.removeSelectionChangedListener(selectionChanged)

    function selectionChanged() {
      const selection = window.getSelection()
      setNumSelectionChanges(numSelectionChanges + 1)
    }
  }, [richTextContext, numSelectionChanges, setNumSelectionChanges])

  return (
    <div>
      Within the rich text editor, the selection has changed {numSelectionChanges} times.
    </div>
  )
}
```

## API
Since the value provided by RichTextContext is exported from bandicoot, it is considered part of bandicoot's public API.
Changes to RichTextContext will follow semantic versioning rules.

The value provided is an object with the following properties:

### Blur
- `selectRangeFromBeforeBlur()`: A function that you should call when you want to ensure that the rich text editor is focused and contains
  the current [selection](/concepts/selection.md). If a blur event has occurred and the rich text editor is no longer focused, this will
  select the text that was selected before the blur.
- `addBlurListener(listener)`: A function that must be given a listener function as an argument. The `listener` will be called whenever
  the rich text editor's [content editable](/concepts/content-editable.md) element is blurred.
- `removeBlurListener(listener)`: A function that must be given a listener function as an argument. The `listener` will be removed so that
  it is no longer called when blur events occur.
- `fireBlur()`: A function that is called by RichTextEditor to notify all listeners that the [content editable](/concepts/content-editable.md)
  element has blurred. This function should probably not be called by any code except the RichTextEditor.
- `isFocused()`: A function that can be called to learn if the rich text editor is currently focused or not. Returns a boolean.

### HTML

- `addNewHTMLListener(listener)`: A function that must be given a listener function as an argument. The `listener` will be called with no
  arguments whenever the rich text editor is forcibly updated to have new HTML content (via `editorRef.current.setHTML()`). To access the
  new HTML, use `richTextContext.getContentEditableElement().querySelector('.thing-im-interested-in')`. This is how you can control
  deserialization, and it is often used in conjunction with `addSerializer`.
- `removeNewHTMLListener(listener)`: A function that must be given a listener function as an argument. The `listener` will be removed
  and no longer called when there is no HTML.
- `fireNewHTML()`: A function that is called by RichTextEditor to notify all listeners that new html has been forcibly set for the editor. This function should probably not be called by any code except for the RichTextEditor itself.
- `getContentEditableElement()`: A function that will return the content editable dom element that was rendered by RichTextEditor.
  Be careful with this one -- it is needed to implement things but also gives you access to the entire rich text dom which means that
  you could break not only your own componet/hook, but also other components and hooks.

### Selection change listeners
- `addSelectionChangedListener(listener)`: A function that must be given a listener function as an argument. The `listener` will be called
  whenever the selection changes **within the rich text editor**. Selection changes outside of the rich text editor will be ignored.
- `removeSelectionChangedListener(listener)`: A function that must be given a listener function as an argument. The `listener` will be removed
  so that it is no longer called when the selection changes.
- `fireSelectionChanged()`: A function that is called by RichTextEditor to notify all listeners of a selection change within the editor. This function should probably only be called by RichTextEditor, not by custom hooks or user logic.

### Serializers
- `addSerializer(serializer)`: A function that must be given a serializer function as an argument. A serializer is a function that will be called
  with a DOM element right before the dom is turned into an HTML string. The serializer should modify the dom element however it would like to
  in preparation for the rich text html to be saved. This is often used in conjunction with `addNewHTMLListener`, which allows you to control
  deserialization.
- `removeSerializer(serializer)`: A function that must be given a serializer function as an argument. The provided serializer will be
  removed so that it is no longer called when html is about to be serialized.
- `numSerializers()`: A function that will return the number of currently active serializers. This is used by RichTextEditor and probably
  doesn't need to be used by any other code.
- `serialize(dom)`: A function that must be given a DOM node as an argument. This function is called by RichTextEditor and returns the DOM
  element that has been modified. The only code that should call this function is RichTextEditor itself.
