# Serialization

> It's got corn for crunch, oats for punch, and it stays crunchy, even in milk.
>
> &#8212; Cap'n Crunch

## Definition

Serialization is the programming concept of turning objects into strings. In the context of bandicoot, serialization refers
to turning the rich text editor DOM elements into an HTML string (so that you can be save it to a database).

Deserialization is the converse - turning strings into objects. In bandicoot, deserialization refers turning an html string
into DOM elements.

## What this means for you

### Serialization
Bandicoot currently only supports serialization to an HTML string. Markdown format is not baked into bandicoot, although there
are html-to-markdown libraries on NPM that can be used in conjunction with bandicoot.

Bandicoot uses `domElement.innerHTML` to serialize the dom elements rich text editor to an HTML string. The serialization can be
customized with [`richTextContext.addSerializer()`](/context/rich-text-context.md). Serialized html strings are then given as an argument
to RichTextEditor's `save()` prop.

### Deserialization
Bandicoot accomplishes deserialization via React's [dangerouslySetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml).
React calls it dangerous for good reason -- if the HTML string has a `<script>` element or other javascript embedded into the dom elements,
that javascript will be executed during deserialization. This is a security risk known as
[cross site scripting (XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting).

Cross site scripting is something you need to avoid in your code. To do so, the HTML string needs to be
[sanitized](https://en.wikipedia.org/wiki/HTML_sanitization). Bandicoot does not currently automatically sanitize your HTML
before using dangerouslySetInnerHTML. However, [RichTextEditor](/components/rich-text-editor.md) does accept a prop `sanitizeHTML` that will be invoked on any HTML that is inserted into the DOM. Please use a library like [sanitize-html](https://github.com/punkave/sanitize-html) in the creation of the sanitization function you provide `sanitizeHTML`.