# Sanitization

> I wish it were Santa-ization.  Who wouldn't like that?
>
> &#8212; Anonymous

## Definition

[Sanitization](https://en.wikipedia.org/wiki/HTML_sanitization) within the context of Bandicoot refers to cleaning an HTML string to remove potentially malicious content. Unsanitized HTML is vulnerable to [cross site scripting (XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting) which is something you need to avoid in your code. Bandicoot does **NOT** automatically sanitize your HTML, however it provides a way for you to bring your own sanitizing function to the party.

## Why should you care?

After receiving incoming HTML, Bandicoot inserts the content into the DOM via React's [dangerouslySetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml).
React calls it dangerous for good reason -- if the HTML string has a `<script>` element or other javascript embedded into the dom elements,
that javascript will be executed during deserialization. This is a security risk known as
[cross site scripting (XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting).

## What this means for you

### Sanitization
Since Bandicoot does **NOT** automatically sanitize incoming or outgoing HTML, it is up to you as the developer/consumer to make sure that sanitization happens. However, Bandicoot does expose a [sanitizeHTML prop](/components/rich-text-editor.md#props) on the [RichTextEditor component](/components/rich-text-editor.md) which allows you to provide a sanitization function.

If a sanitization function is provided through the `sanitizeHTML` prop, Bandicoot will pass incoming html strings through this function before inserting the content into the DOM (see [note 1](/concepts/sanitization.md#notes)). If provided, Bandicoot will also pass outgoing html through the `sanitizeHTML` function after serialization (see [note 2](/concepts/sanitization.md#notes)). If a `sanitizeHTML` function is not provided, Bandicoot will simply pass through the *unsanitized* html string - which is **NOT RECOMMENDED**!

### The `sanitizeHTML` Function 
When Bandicoot calls the `sanitizeHTML` function it passes two arguments:
1. `suspectHTML`: An string of html content that potentially needs sanitizing.
2. `actionType`: A string indicating what action Bandicoot is about to take.

Bandicoot expects your `sanitizeHTML` function to return the sanitized html string.

The `actionType` options and when they occur are as follows:
- `setHTML`: Occurs right before inserting the `initialHTML` into the DOM as well as right before inserting content into the DOM through the `editorRef.current.setHTML()` and `editorRef.current.resetEditor()` functions.
- `pasteHTML`: Occurs right before passing a paste operation's content to the [pasteFn prop](/components/rich-text-editor.md#props).  Note that the returned html string from the `pasteFn` function is immediately inserted into the DOM without further sanitization.
- `insertContentEditableFalseHTML`: Occurs right before inserting content into the DOM using the `useContentEditableFalse` hook's `insertContentEditableFalseElement` function.
- `getHTML`: Occurs post serialization, right before returning content through the `save()` or `editorRef.current.getHTML()` functions.
- `initialSetLastSavedHTML`: Occurs right before initially setting the RichTextEditor's internal lastSavedHTML state variable.

### Example
```js
<RichTextEditor
  sanitizeHTML={myOwnSanitizeHtmlFunction}
/>

function myOwnSanitizeHtmlFunction(suspectHTML, actionType) {
  // Implement your logic to sanitize 'suspectHTML' into 'sanitizedHTML'.
  // If desired, key off of the 'actionType' string.
  return sanitizedHTML;
}
```

## Notes
1) If you use Bandicoot's `richTextContext.getContentEditableElement()` to interact with the HTML directly, be aware that the content does not pass through the `sanitizeHTML` function since you are accessing the DOM directly.

2) Content being sent to a server from a client should never be considered sanitized since a bad actor could tamper with client-side code or bypass sanitization completely by sending malicious data directly to a server's endpoints. Server-side incoming sanitization should be the primary line of defense instead of client-side outgoing sanitization.