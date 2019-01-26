# Uncontrolled editor
> Philosophers have examined for centuries the now infamous paradox where control buttons can coexist with
> uncontrolled editors.
>
> No progress has been made.
>
> &#8212; Anonymous

## Definition

Bandicoot's [RichTextEditor](/components/rich-text-editor.md) component acts like a
[React uncontrolled component](https://reactjs.org/docs/uncontrolled-components.html).

What this means is that bandicoot does not have an onChange listener for the
[content editable div](/concepts/content-editable.md). Additionally the innerHTML of the
content editable div is not set by Bandicoot every render. Instead, the browser is the
only thing that keeps track of what is inside of the editor.

## What this means for you
There is no json state that defines the value of your rich text editor. Instead, only the DOM and HTML keeps track of that.
This means that you should be storing (or converting to/from) HTML whenever interacting with bandicoot. This does not preclude
conversion to markdown, but just means that there isn't a JSON string you'll have to store in the database on top of that.

## Implications for bandicoot
To my knowledge, bandicoot is be the only rich text editor library that uses an uncontrolled content editable element.
Other rich text libraries maintain javascript state that mirrors what is in the DOM.

One big advantage of treating the editor as an uncontrolled component is that the bandicoot library doesn't have to do as much work.
It defers much of the heavy lifting of rich text editing to the browser, including undo/redo, inserting `<b>` for bolding, keyboard
shortcuts, and more. Because of this, the bandicoot library is [significantly smaller](/README.md#comparison-to-other-rich-text-libraries)
than other javascript rich text libraries.

A disadvantage of using an uncontrolled component is that we're more at the mercy each browser's implementation of rich text.
This used to be a showstopper in the Internet Explorer (and older browser) age, since support for many rich text features
was spotty.
