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
Bandicoot accomplishes deserialization via React's [dangerouslySetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml). For this reason, it is **HIGHLY RECOMMENDED** that you review the [sanitization docs](/concepts/sanitization.md) to learn more about what you need to do to avoid security vulnerabilities.