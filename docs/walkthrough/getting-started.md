# Getting started
This tutorial will show you which components you need to render for your rich text editor. Then it will show how to implement bolding.

## Installation
```sh
npm install --save bandicoot
# Or
yarn add bandicoot
```

## Usage
The bandicoot library exports two components, both of which must be rendered for rich text editing.

```jsx
import {RichTextContainer, RichTextEditor} from 'bandicoot'

function MyComponent() {
  return (
    <RichTextContainer>
      <RichTextEditor />
    </RichTextContainer>
  )
}
```

This will give you a [contentEditable div](/concepts/content-editable.md) that is ready for rich text editing.
At this point, the user can control the rich text via keyboard shortcut keys (such as Ctrl + B for bold),
but there won't be any [control buttons](/concepts/control-button.md) that allow them to click on a button
in order to modify the styling of the text. Additionally, the user will be able to paste text and html into the editor.
