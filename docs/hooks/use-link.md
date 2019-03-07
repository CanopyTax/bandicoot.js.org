# useLink

The `useLink` hook provides functionality for inserting, modifying, and removing links (`<a>` elements) in your rich text editor.

Inserting a link can be a done with a DOM one liner: `document.execCommand('createLink', null, 'https://bandicoot.js.org')`.
However, styling the link, modifying the link, removing the link, or selecting the entire link are much easier to do with some help
from `useLink`.

## Basic Example
```jsx
import {useLink} from 'bandicoot'

function Link() {
  const {insertLink} = useLink()

  return (
    <>
      <button onClick={() => insertLink('https://bandicoot.js.org', 'Bandicoot Docs')}>
        Insert link
      </button>
    </>
  )
}
```

## Advanced Example
```jsx
import {useLink} from 'bandicoot'
import {useState} from 'react'

function Link() {
  const [showingDialog, setShowingDialog] = useState(false)
  const [linkLabel, setLinkLabel] = useState('')
  const [linkUrl, setLinkUrl] = useState('')
  const {insertLink, getTextFromBeforeBlur, selectEntireLink, unlink} = useLink({processAnchorElement})

  return (
    <>
      <button onClick={openDialog}>
        Insert link
      </button>
      <dialog open={showingDialog}>
        <form onSubmit={handleSubmit}>
          <input value={linkLabel} placeholder="Label" onChange={evt => setLinkLabel(evt.target.value)} />
          <input value={linkUrl} placeholder="Label" onChange={evt => setLinkUrl(evt.target.value)} />
          <button type="submit">Insert</button>
          <button type="button" onClick={() => setShowingDialog(false)}>Cancel</button>
        </form>
      </dialog>
    </>
  )

  function openDialog() {
    setLinkLabel(getTextFromBeforeBlur())
    setShowingDialog(true)
  }

  function handleSubmit() {
    insertLink(linkUrl, linkLabel)
    setLinkLabel('')
    setLinkUrl('')
    setShowingDialog(false)
  }

  function processAnchorElement(anchorElement) {
    anchorElement.addEventListener('click', () => {
      if (window.prompt('Are you sure that you want to remove this link?')) {
        selectEntireLink(anchorElement)
        unlink()
      }
    })
  }
}
```

## API
```js
const {insertLink, getTextFromBeforeBlur, selectEntireLink, unlink} = useLink({processAnchorElement})
```

### Arguments
The `useLink` hook optionally takes in one object argument with the following properties:
- `processAnchorElement` (optional): A function that will be called with each `<a>` dom element in the rich text editor. This can be used
  to set up click listeners, change the style, etc. The function will be called for anchor elements in the initialHTML, calls to setHTML, and also for
  every anchor element that is inserted via `useLink()`. Defaults to a no-op function.

### Return value
The `useLink` hook returns an object with the following properties:
- `insertLink(url, label)`: A function that inserts a link into the rich text editor. This function requires two arguments: `url` and `label`.
  Both arguments must be strings.
- `unlink()`: A function that will remove the link from the selected text in the rich text editor. This is often used in conjunction with
  `selectEntireLink`.
- `selectEntireLink(anchorElement)`: A function that will select (highlight) the entire link in the rich text editor. This is useful for
  making sure you unlink the entire link, instead of just portions of it. An anchor element must be passed to it.
- `getTextFromBeforeBlur()`: A function that will give you the most recently selected or highlighted text inside of the rich text editor.
  This is useful for setting the default label for a link that the user is wanting to insert.

## Notes
- `useLink` uses [`useDocumentExecCommand`](/hooks/use-document-exec-command.md) underneath the hood to call `document.execCommand()`.
