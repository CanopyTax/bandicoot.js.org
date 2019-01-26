# Bold, Italic, and Underline

Let's add a [control button](/concepts/control-button.md) for bolding the rich text:
```jsx
import {RichTextContainer, RichTextEditor, useDocumentExecCommand, useDocumentQueryCommandState} from 'bandicoot'

function MyComponent() {
  return (
    <RichTextContainer>
      <Bold />
      <RichTextEditor />
    </RichTextContainer>
  )
}

function Bold() {
  const {performCommand} = useDocumentExecCommand('bold')
  const {isActive} = useDocumentQueryCommandState('bold')

  return (
    <button
      onClick={performCommand}
      className={isActive ? 'active-control-button' : ''}
    >
      Bold
    </button>
  )
}
```

Now we have a bold button that, when clicked, will toggle the highlighted text to either be bolded or not bolded. Additionally, when the
user selects a bolded part of the editor, the bold button will have an extra css class on it to indicate this to the user. Notice that
bandicoot did not render the button itself, but just provided a `performCommand` function and an `isActive` boolean for us to
use on a button that we render on our own. This allows us to position and style the Bold button however we'd like to, without having
to worry about the DOM interactions for bolding/unbolding.

This use of [React hooks](https://reactjs.org/docs/hooks-intro.html) is the pattern that bandicoot employs whenever you need to implement a new
"control button". Italics, underlining, font size, links, images, and read-only elements can all be achieved by importing a bandicoot hook and using the hook inside of
your component. Check out the docs for [useDocumentExecCommand](/hooks/use-document-exec-command.md)
and [useDocumentQueryCommandState](hookes/use-document-query-command-state.md) for more information on the two hooks used here.

All of bandicoot's hooks are optional to use and can be composed to create a wide variety of rich text features. The hooks are meant to hide the ugly
DOM stuff from you so that you can focus on the features of your rich text editor instead.


## Now for italic and underline
Italic and underline can be implemented by copying our Bold component with only minor modifications:
```jsx
function Italic() {
  const {performCommand} = useDocumentExecCommand('italic')
  const {isActive} = useDocumentQueryCommandState('italic')

  return (
    <button
      onClick={performCommand}
      className={isActive ? 'active-control-button' : ''}
    >
      Italic
    </button>
  )
}

function Underline() {
  const {performCommand} = useDocumentExecCommand('underline')
  const {isActive} = useDocumentQueryCommandState('underline')

  return (
    <button
      onClick={performCommand}
      className={isActive ? 'active-control-button' : ''}
    >
      Underline
    </button>
  )
}
```
