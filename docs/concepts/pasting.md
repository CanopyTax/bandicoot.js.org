# Pasting

> Has anybody noticed that pasting is sorta like pasta?
>
> &#8212; Anonymous

## Definition
Pasting is when the user attempts to insert content into their rich text editor with Ctrl/Cmd + V
or when code attempts to insert content via `document.execCommand('paste')`.

[Read more on MDN](https://developer.mozilla.org/en-US/docs/Web/Events/paste)

## What this means for you
Bandicoot by default will allow for any and all pasting. If you wish to customize which pastes are allowed, you can 
provide the [pasteFn prop](/components/rich-text-editor.md#props).

Note that pasted html is vulnerable to [cross site scripting (XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting)
and so you should also provide the `sanitizeHTML` to prop `RichTextEditor` to [sanitize the HTML](https://en.wikipedia.org/wiki/HTML_sanitization).