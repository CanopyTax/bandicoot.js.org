# Bandicoot
Bandicoot is an npm library for doing rich text with React. It uses [React hooks](https://reactjs.org/docs/hooks-intro.html), is
only 3.8kb gzipped (12kb ungzipped), and will keep you focused on writing your react components instead of grappling with your rich text editor.

[Walkthrough](/walkthrough/getting-started.md)

[Components](/components/rich-text-editor.md)

[Hooks](/hooks/use-document-exec-command.md)

[Concepts](/concepts/README.md)

## Browser support
Bandicoot has been tested in Safari 10+, Edge 14+, Chrome, and Firefox. It does not support IE11.

## Comparison to other rich text libraries
There are many free rich text libraries, such as [Quill](https://quilljs.com/), [Slate](https://github.com/ianstormtaylor/slate),
[tinymce](https://www.tiny.cloud/), and [DraftJS](https://draftjs.org/).

Here's a very incomplete comparison:

|           | Library Size (not gzipped)                  | License / Price | Inline images | Links           | "Atomic blocks" | Built-in markdown support
|-----------|---------------------------------------------|-----------------|---------------|-----------------|-------------------|--------------------------
| Bandicoot | [12kb](https://unpkg.com/bandicoot/dist)    | MIT (free)      | &#10004;      | &#10004;        | &#10004;          | &#10008;
| Quill     | [215kb](https://unpkg.com/quill/dist/)      | MIT (free)      | &#10004;      | &#10004;        | &#10008;          | &#10004;
| Slate     | [171kb](https://unpkg.com/slate/dist/)      | BSD-3 (free)    | &#10004;      | &#10004;        | &#10004;          | &#10004;
| TinyMCE   | [353kb](https://unpkg.com/tinymce/)         | GPL 2.1 ([pay for plugins, support, storage, and no product attribution](https://www.tiny.cloud/pricing/))| &#10004;      | &#10004;        | &#10004;        | &#10004;
| DraftJS   | 210kb [Draft](https://unpkg.com/draft-js/dist/) + [Immutable](https://unpkg.com/immutable/dist/) | MIT (free) | &#10004;      | &#10004;        | &#10004;        | &#10008;

## Bandicoot's take
Bandicoot should enable developers to build an editor that (1) does what they need, (2) doesn't require 200kb of code, and (3) can actually be
understood, modified, and debugged. Abstraction is a double edged sword: the ideal rich text library is one
where there are building blocks that developers can understand.

I've used rich text libraries for several years, and have always run into painful roadblocks. Bugs are often difficult to
diagnose and nearly impossible to fix. Documentation seems good until I hit something I couldn't find in it. I am happy for
library abstractions until I find myself reading the source code and wishing I was working with the DOM instead of the library.
Storing HTML in the database seems fine but then I learn that the JSON representation is more important. Updating versions of
a library is often painful. Browsers support rich text editing a lot better than they did ten years ago, but rich
text libraries today are still just as complicated as they were then.

Bandicoot attempts to address those problems by using lightweight abstractions on top of existing DOM APIs.
Rich text is inherently complicated, but the library you use for it
shouldn't make it more complicated.

[See walkthrough](/walkthrough/getting-started.md)
