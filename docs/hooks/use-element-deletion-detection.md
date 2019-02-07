# useElementDeletionDetection

If you wish to be notified when a rich text dom element is deleted, you can do so with the `useElementDeletionDetection`. This can be helpful
if you have added interactivity to rich text elements such as links, images, or other rich text elements that should be cleaned up once
the rich text element is deleted from the editor.

## Example
In this example, we'll show a popup when you click on a link in the rich text editor and will use the `useElementDeletionDetection` hook
to hide the popup if the link gets removed while the popup is open.

```js
import React, {useState} from 'react'
import {useLink, useElementDeletionDetection} from 'bandicoot'

function LinkButton(props) {
  const [anchorElementForPopup, setAnchorElementForPopup] = useState(false)
  useLink({processAnchorElement})
  useElementDeletionDetection(anchorElementForPopup, closePopup)

  return showPopup && (
    <div className="popup">
      A popup for the link
    </div>
  )

  function processAnchorElement(anchorElement) {
    anchorElement.addEventListener('click', () => {
      setAnchorElementForPopup(anchorElement)
    })
  }

  function closePopup() {
    setAnchorElementForPopup(null)
  }
}
```

## API
```js
useElementDeletionDetection(domElement, callback)
```

### Arguments
- `domElement`: A domElement that bandicoot will check to see if it has been removed from the dom. If you pass in a falsy value such as null, `useElementDeletionDetection` will do nothing.
- `callback`: A function that will be called exactly once when the dom element is removed from the DOM (and rich text editor).
