# RichTextEditor

The RichTextEditor component is responsible for rendering a [content editable](/concepts/content-editable.md) element.
Additionally it provides the following features:
- Allows you to set an initial value for the rich text
- Allows you to forcibly update the html in your rich text editor
- Notifies you when it's time to save your rich text
- Fires events to your [control buttons](/concepts/control-button.md) when the selection changes, the editor blurs,
  and more. See the [docs for RichTextContext](/context/rich-text-context.md) for more details.

Note that the RichTextEditor must be rendered inside of a [RichTextContainer](/components/rich-text-container.md) component.
This is because RichTextContainer [provides React context](https://reactjs.org/docs/context.html#contextprovider) that is
consumed by RichTextEditor.

## Basic Example
```jsx
import {RichTextEditor, RichTextContainer} from 'bandicoot'

function MyEditor() {
  return (
    <RichTextContainer>
      <RichTextEditor />
    </RichTextContainer>
  )
}
```

### Advanced Example
```jsx
import {useRef} from 'react'
import {RichTextEditor, RichTextContainer} from 'bandicoot'

function MyEditor() {
  const editorRef = useRef(null)

  return (
    <RichTextContainer>
      <button onClick={resetEditor}>
        Reset editor
      </button>
      <button onClick={forceSetHTML}>
        Force set html
      </button>
      <RichTextEditor
        ref={editorRef}
        initalHTML={props.htmlSavedInDatabase}
        save={save}
        unchangedInterval={3000}
        className="my-editor"
        sanitizeHTML={props.myOwnSanitizationFunction}
      />
    </RichTextContainer>
  )

  function save(newHTML) {
    // Will be called on blur or after unchangedInterval
    fetch('/api-endpoint', {
      method: 'PATCH',
      body: JSON.stringify({
        richText: newHTML
      })
    })
  }

  function resetEditor() {
    editorRef.current.resetEditor()
  }

  function forceSetHTML() {
    editorRef.current.setHTML(`<span>Some new html for the editor</span>`)
  }
}
```

## API

### Props
- `initialHTML` (optional): A string of html that will be the editor's initial value. Defaults to empty string.
- `save` (optional): A function that will be called whenever the editor blurs or no changes are made for an `unchangedInterval`.
  The `save` function will be called with one argument, `html`, that is a serialized html string. Defaults to no-op.
- `unchangedInterval` (optional): An integer number of milliseconds that will be waited before calling `save()`. Whenever
    the editor's value hasn't been saved and hasn't changed for `unchangedInterval` milliseconds, the `save()` function will be called.
    Defaults to `null`, which means that the `save()` function will only be called on blur.
- `className` (optional): A string className that will be applied to the [content editable](/concepts/content-editable.md) element.
- `style` (optional): An object of css styles to apply to the [content editable](/concepts/content-editable.md) element.
- `placeholder` (optional): A string placeholder that will be shown when the rich text editor is empty.
- `placeholderColor` (optional): A string css color for the placeholderText. Defaults to `#CFCFCF`.
- `pasteFn` (optional): A function that will be called whenever the user pastes to the editor. The function will be called with a string of text and
    returns a string that will be pasted. If you wish to prevent a paste entirely, return `false` from the pasteFn. Note that pasted HTMl is vulnerable
    to cross site scripting. See [pasting docs](/concepts/pasting.md) for more details.
- `sanitizeHTML` (optional but **HIGHLY RECOMMENDED**): A function that will be called before Bandicoot inserts a received html string into the DOM or before Bandicoot sends the DOM content out as an html string after serialization. If no function is provided, Bandicoot will simply pass along an *unsanitized* html string. See [sanitization docs](/concepts/sanitization.md) for more information.

### Ref
In addition to passing props, you can control the RichTextEditor imperatively with a [React ref](https://reactjs.org/docs/glossary.html#refs).
The ref has the following properties:
- `setHTML(html)`: A function that will forcibly set the editor to have the new `html` string as its value.
- `getHTML()`: A function that returns serialized HTML as a string. The `html` returned is the same as what is given to the `props.save` function.
- `resetEditor()`: A function that you can call to empty out the editor. This is equivalent to calling `editorRef.current.setHTML('')`.
- `focus()`: A function that you can call to forcibly focus the editor.