# H1 with formatBlock

NOTE: It is not recommended to attempt h1/h2/h3 tags given [the different browsers' inability to properly handle headers inside of (un)ordered lists](https://github.com/CanopyTax/bandicoot/issues/53). Until at least Chrome can correctly [toggle header blocks in lists](https://stackoverflow.com/questions/57468855/how-can-i-toggle-contenteditable-h1-inside-of-ordered-lists-in-chrome) there is little we can do. [Manual DOM manipulation](https://github.com/CanopyTax/bandicoot/pull/54) proves unsuccessful.

Let's add a [control button](/concepts/control-button.md) for adding a block-level element around the line containing the current selection:
```jsx
import {RichTextContainer, RichTextEditor, useDocumentExecCommand, useFormatBlock} from 'bandicoot'

function MyComponent() {
  return (
    <RichTextContainer>
      <HeaderButton value='h1'/>
      <RichTextEditor />
    </RichTextContainer>
  )
}

function HeaderButton(props) {
  const value = {props};
  const {formatBlock} = useFormatBlock()
  const {isActive} = useDocumentQueryCommandState('formatBlock', value)

  return (
    <button
      onClick={formatBlock(value)}
      className={isActive ? 'active-control-button' : ''}
    >
      H1
    </button>
  )
}
```

Now we have a button that, when clicked, will toggle an h1 wrapper around the line or selection (if highlighted). Notice that
bandicoot did not render the button itself, but just provided a `useFormatBlock` function and an `isActive` boolean for us to
use on a button that we render on our own. This allows us to position and style the H1 button however we'd like to, without having to worry about the DOM interactions for formatBlock.

This use of [React hooks](https://reactjs.org/docs/hooks-intro.html) is the pattern that bandicoot employs whenever you need to implement a new"control button". Italics, underlining, font size, links, images, and read-only elements can all be achieved by importing a bandicoot hook and using the hook inside of your component. Check out the docs for [useFormatBlock](/hooks/use-format-block.md)
and [useDocumentQueryCommandState](hookes/use-document-query-command-state.md) for more information on the two hooks used here.

All of bandicoot's hooks are optional to use and can be composed to create a wide variety of rich text features. The hooks are meant to hide the ugly DOM stuff from you so that you can focus on the features of your rich text editor instead.
