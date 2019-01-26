# useImage

The `useImage` hook makes it easy to implement inline images into your rich text editor.

The DOM already supports inline images with a one-liner:
[`document.execCommand('insertImage', null, 'https://mycdn.com/path-to-image.png')`](https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand#Commands).
Unfortunately, though, it still can be hard to choose an image, create a url for the image, style the image, or remove an image.
Which is where the `useImage` hook helps out.

## Basic Example
```jsx
import {useImage} from 'bandicoot'

function Image() {
  const {chooseFile} = useImage()

  return (
    <button onClick={chooseFile}>
      Insert Image from Computer
    </button>
  )
}
```

## Advanced Example
```jsx
import {useImage} from 'bandicoot'

function RemovableImage() {
  const {chooseFile, removeImage} = useImage({processImgElement, fileBlobToUrl})

  return (
    <button onClick={chooseFile}>
      Insert Image from File
    </button>
  )

  function processImgElement(imgElement) {
    // Every time an image gets inserted, this function gets called with the DOM img element
    imgElement.addEventListener('click', () => {
      if (window.prompt('Are you sure you want to remove this image?')) {
        removeImage(imgElement)
      }
    })
  }

  function fileBlobToUrl(fileBlob) {
    const formData = new FormData()
    formData.append('files', fileBlob)

    // We need to return a promise that resolves with a string url that the <img> element can use.
    return fetch('/server-api-that-handles-file-uploads', {
      method: 'POST',
      body: formData,
    })
    .then(response => response.json())
    .then(json => json.newlyCreatedFileUrl)
  }
}
```

## API
```js
const {chooseFile, removeImage} = useImage({processImgElement, fileBlobToUrl})
```

### Arguments
`useImage` can be called with no arguments (to use the defaults) or with a single object argument with the following properties:

- `processImgElement` (optional): A function that will be called whenever an img element is inserted into the rich text editor. The `processImgElement`
  will be called with one argument, the `imgElement`, which is the DOM `<img>` element that has just been inserted. You can then do what
  you want with it, including setting up click listeners, changing the styling, etc. Defaults to no-op.
- `fileBlobToUrl` (optional): A function that will be called whenever the user has picked a file from their computer to insert into the rich text
  editor. The `fileBlobToUrl` function will be called with one argument, a [`File`](https://developer.mozilla.org/en-US/docs/Web/API/File).
  The return value of `fileBlobToUrl` must be a promise that resolves with a string url for that FileBlog object. This is useful for uploading a
  file to a server before inserting it into the DOM.

## Return value
`useImage` returns an object with the following properties:
- `chooseFile()`: A function that will open up a file picker and allow the user to choose a file from their computer. When the user selects an image,
  that image will be inserted into the RichTextEditor.
- `removeImage(imgElement)`: A function that will remove an image from the rich text editor while preserving undo/redo state. This function must be called with
  one argument, the `<img>` dom element that you wish to remove.

## Notes
- `useImage` uses [`useDocumentExecCommand`](/hooks/use-document-exec-command.md) underneath the hood to call `document.execCommand()`.
