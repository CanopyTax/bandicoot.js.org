# H1 with formatBlock

Let's add a [control button](/concepts/control-button.md) for adding a block-level element around the line containing the current selection:
```jsx
import {RichTextContainer, RichTextEditor, useDocumentExecCommand, useDocumentQueryCommandState} from 'bandicoot'

function MyComponent() {
  return (
    <RichTextContainer>
      <H1 value='<h1>'/>
      <RichTextEditor />
    </RichTextContainer>
  )
}

function H1(props) {
  const value = {props};
  const {performCommandWithValue} = useDocumentExecCommand('formatBlock')
  const {isActive} = useDocumentQueryCommandState('formatBlock', 'h1')

  return (
    <button
      onClick={performCommandWithValue(value)}
      className={isActive ? 'active-control-button' : ''}
    >
      Bold
    </button>
  )
}
```

Now we have a button that, when clicked, will toggle an h1 wrapper on the line of selection. Notice that
bandicoot did not render the button itself, but just provided a `performCommand` function and an `isActive` boolean for us to
use on a button that we render on our own. This allows us to position and style the H1 button however we'd like to, without having
to worry about the DOM interactions for formatBlock.

This use of [React hooks](https://reactjs.org/docs/hooks-intro.html) is the pattern that bandicoot employs whenever you need to implement a new
"control button". Italics, underlining, font size, links, images, and read-only elements can all be achieved by importing a bandicoot hook and using the hook inside of
your component. Check out the docs for [useDocumentExecCommand](/hooks/use-document-exec-command.md)
and [useDocumentQueryCommandState](hookes/use-document-query-command-state.md) for more information on the two hooks used here.

All of bandicoot's hooks are optional to use and can be composed to create a wide variety of rich text features. The hooks are meant to hide the ugly
DOM stuff from you so that you can focus on the features of your rich text editor instead.
