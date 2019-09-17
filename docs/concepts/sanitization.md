# Sanitization

> I wish it were Santa-ization.  Who wouldn't like that?
>
> &#8212; Anonymous

## Definition

[Sanitization](https://en.wikipedia.org/wiki/HTML_sanitization) within the context of Bandicoot refers to cleaning an HTML string to remove potentially malicious content. Unsanitized HTML is vulnerable to [cross site scripting (XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting) which is something you need to avoid in your code. Bandicoot does **NOT** automatically sanitize your HTML, however it provides a way for you to bring your own sanitizing function to the party.

## Why you should care

After receiving incoming HTML, Bandicoot inserts the content into the DOM via React's [dangerouslySetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml).
React calls it dangerous for good reason -- if the HTML string has a `<script>` element or other javascript embedded into the dom elements,
that javascript will be executed during deserialization. This is a security risk known as
[cross site scripting (XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting).

## What this means for you

### Sanitization
Since Bandicoot does **NOT** automatically sanitize incoming or outgoing HTML, it is up to you as the developer to make sure that sanitization happens. However, Bandicoot does expose a [sanitizeHTML prop](/components/rich-text-editor.md#props) on the [RichTextEditor component](/components/rich-text-editor.md) which allows you to provide a sanitization function.

If a sanitization function is provided through the `sanitizeHTML` prop, Bandicoot will pass incoming HTML strings through this function before inserting the content into the DOM (see [note 1](/concepts/sanitization.md#notes)). If provided, Bandicoot will also pass outgoing HTML through the `sanitizeHTML` function after serialization (see [note 2](/concepts/sanitization.md#notes)). If a `sanitizeHTML` function is *NOT* provided, Bandicoot will simply pass through the *unsanitized* HTML string. In the case of inserting the content into the DOM this would mean using `dangerouslySetInnerHTML` with unsanitized HTML - **NOT RECOMMENDED**!

Ultimately, the choice on what sanitization process to take or what library to use is up to you. If you are looking to do client-side sanitization, there are many [third-party HTML sanitizers](https://www.npmjs.com/search?q=html+sanitizer) that already exist. One that works well is [sanitize-html](https://github.com/apostrophecms/sanitize-html#readme).

### The `sanitizeHTML` function 
When Bandicoot calls the `sanitizeHTML` function it passes two arguments:
1. `suspectHTML`: An string of HTML content that potentially needs sanitizing.
2. `actionType`: A string indicating what action Bandicoot is about to take.

The `actionType` options and when they occur are as follows:
- `setHTML`: Occurs right before inserting the `initialHTML` into the DOM as well as right before inserting content into the DOM through the `editorRef.current.setHTML()` and `editorRef.current.resetEditor()` functions.
- `pasteHTML`: Occurs right before passing a paste operation's content to the [pasteFn prop](/components/rich-text-editor.md#props).  Note that the returned HTML string from the `pasteFn` function is immediately inserted into the DOM without further sanitization.
- `insertContentEditableFalseHTML`: Occurs right before inserting content into the DOM using the `useContentEditableFalse` hook's `insertContentEditableFalseElement` function.
- `getHTML`: Occurs post serialization, right before returning content through the `save()` or `editorRef.current.getHTML()` functions.
- `initialSetLastSavedHTML`: Occurs right before initially setting the RichTextEditor's internal lastSavedHTML state variable.

Bandicoot expects your `sanitizeHTML` function to return the sanitized HTML string.

### Example
```js
import sanitizeHTML from 'sanitize-html'
import {RichTextEditor, RichTextContainer} from 'bandicoot'

function MyEditor() {
  return (
    <RichTextContainer>
      <RichTextEditor sanitizeHTML={sanitizeHTML} />
    </RichTextContainer>
  )
}

function myOwnSanitizeHtmlFunction(suspectHTML, actionType) {
  // Implement your logic to sanitize 'suspectHTML' into 'sanitizedHTML'.
  // If desired, key off of the 'actionType' string.  For example:
  // - if 'pasteHTML', sanitize using rules X, Y, and Z.
  // - else, sanitize using just rules X and Y.
  return sanitizedHTML;
}
```

## Notes
1) If you use Bandicoot's `richTextContext.getContentEditableElement()` to interact with the HTML directly, be aware that the content does not pass through the `sanitizeHTML` function since you are accessing the DOM directly.

2) Content being sent to a server from a client should never be considered sanitized since a bad actor could tamper with client-side code or bypass sanitization completely by sending malicious data directly to a server's endpoints. Server-side incoming sanitization should be the primary line of defense instead of client-side outgoing sanitization.