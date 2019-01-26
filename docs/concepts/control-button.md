# Control Button
> It's not often that there is an actual button that will give you control.
> So when there is, you better press it.
>
> &#8212; Anonymous

## Definition

A control button is a button in charge of modifying the rich text through user interaction. For example,
a "B" button icon is often used as the control button for bolding.

## Example
See this [walkthrough example](/walkthrough/bold-italic-underline.md) for how to create your own control buttons.

## Explanation

Control buttons are not a DOM concept, but a bandicoot concept. In bandicoot editors, the control buttons
often handle the following things:

1. Allow the user to modify or insert rich text through user interactions (e.g., clicking on a Bold button).
2. Indicate to the user if the control button is "active" for currently selected text (e.g., change the
Bold button's color when the currently selected text is bold).
3. Handle serialization of inserted and modified rich text so that the correct html string is stored in a database.
4. Handle deserialization of rich text so that the correct dom nodes exist in the rich text editor when loading from
a database html string.

To accomplish these things, control buttons use [bandicoot hooks](/hooks/use-document-exec-command.md). Code that
uses bandicoot is in charge of rendering the button and handling the user interaction, but when it's time to perform
the rich text logic, a bandicoot hook will actually make the changes to the rich text.

Control buttons in bandicoot are different than in other rich text libraries because of #3 and #4. Often, rich text libraries
will split #1 and #2 from #3 and #4 so that there is not one component or file that handles all of those things. However,
bandicoot's philosophy is to keep all the logic together for each rich text feature, which means that control buttons
take on more responsibility.

