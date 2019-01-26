# RichTextContainer

The RichTextContainer component [provides React context](https://reactjs.org/docs/context.html#contextprovider) to
your [RichTextEditor](/components/rich-text-editor.md) and [control buttons](/concepts/control-button.md). This allows
the editor and control buttons to communicate with each other while still letting you render your own control buttons.

The RichTextEditor and all control buttons must be rendered as children of a RichTextContainer in order for
bandicoot to work properly.

## Basic Example
```jsx
import {RichTextContainer, RichTextEditor} from 'bandicoot'

function MyEditor() {
  return (
    <RichTextContainer>
      <RichTextEditor />
    </RichTextContainer>
  )
}
```

## Advanced Example
```jsx
import {RichTextContainer, RichTextEditor, useDocumentExecCommand, useDocumentQueryCommandState} from 'bandicoot'

function MyEditor() {
  return (
    <RichTextContainer>
      <div className="control-buttons">
        <Bold />
        <Underline />
      </div>
      <RichTextEditor />
    </RichTextContainer>
  )
}

function Bold() {
  const {performCommand} = useDocumentExecCommand('bold')
  const {isActive} = useDocumentQueryCommandState('bold')

  return (
    <button onClick={performCommand} className={isActive ? 'active-button' : ''}>
      Bold
    </button>
  )
}

function underline() {
  const {performCommand} = useDocumentExecCommand('underline')
  const {isActive} = useDocumentQueryCommandState('underline')

  return (
    <button onClick={performCommand} className={isActive ? 'active-button' : ''}>
      Underline
    </button>
  )
}
```

## API

### Props
The RichTextContainer component does not accept any props.

### Ref
The RichTextContainer does not provide any functionality via a [React ref](https://reactjs.org/docs/glossary.html#refs).
