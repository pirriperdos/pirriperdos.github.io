---
title: Comparison & Observation of Rich Text Editors
---

## Types of text editors
### WYSIWYG
> WYSIWYG editor is a system in which content (text and graphics) can be edited in a form closely resembling its appearance when printed or displayed as a finished product  
> — Wikipedia  
### Structural editor
> A structure editor, also structured editor or projectional editor, is any document editor that is cognizant of the document’s underlying structure.  
> — Wikipedia  
>   
### Plain text editors with live preview
the plain text itself might be parsed into a AST. but you are not editing the AST, but editing that plain text, and you need to take care yourself if the edited result can be parsed or not

examples of this kind is not included in this comparison, these are basically plain text editors, with syntax highlighting. example:  [StackEdit](https://stackedit.io/) 

### differences between WYSIWYG and structural editor
there are editors that are both structural and WYSIWYG. compare them with this example:

* only WYSIWYG (Google Docs): ![](03/Screen%20Shot%202018-05-21%20at%2012.16.49%201.png). you don’t know if you type  a new character, what format will it appear in (but you can if the current format is displayed in like for example a format menu)
* structural (Bear.app): ![](03/Screen%20Shot%202018-05-21%20at%2012.17.18%201.png). you know the cursor format by some additional glyphs that won’t show up when printed
* both (TeXmacs): ![](03/May-21-2018%2012-20-02.gif) it works like the structural one, with a zero-width control glyph

there are also other differences: structural editor usually has some semantical AST construction (heading), while WYSIWYG editor usually has some surface AST construction (font, font size).
but there is no hard rules

### differences between a plain text editor and structural editor

structural editor might has different editing operations you can perform. also the presentation is more “WYSIWYG” than plain text editors, for example, showing a url with no url plain text (but a modal dialog to edit the url), displaying images inline (instead of a url/file name)
## Common data model
our data model will be similar to something like this:
```
Document = [Block]
Block = BlockType1(content: [Inline]) | BlockType2(blocks: [Block])
Inline = PlainText(string: String) | FormatedInline(format: Format, content: [Inline]) | EmbededImage(url: String)
```
so there are `Document` which is a list of **block**s and each block is either again some blocks (for example, `Blockquote(blocks: [Block])`) or a list of **Inline** elements

## Aspects
* type of editor
* underlining tech stack
	* macOS Cocoa text API: provides out of box spell checker, RTL support, multiple selection (by Command and Option keys)
	* web: provides out of box spell checker, RTL support, no multiple selection
	* custom made
* [RTL script](https://en.wikipedia.org/wiki/Right-to-left) support
* selection behavior
    * structural editors might only allow you to select block level items when you across the block boundary
    * some editor support multiple selections
    * (not in our scope, but some plain text editor support multiple cursors)
* copy & paste behavior
    * for macOS for example, when you copy something, you copy the content to each of all available pasteboard, depending on where you are pasting to, a proper pasteboard is selected to paste from it
* mouse free editing
	* text to AST smart conversion
	* keyboard navigation like Vim: most of the candidates has poor support for this
* data model & interoperability with other format 
* tables?

## candidates
* structural
	* TeXmacs
	* Bear.app
* notion.so
* WYSIWYG
	* web based
		* Google Docs
		* ProseMirror
		* https://www.froala.com/wysiwyg-editor
	* Microsoft Word


## TeXmacs
* a structural editor that layout content WYSIWYG

    this is a structural editor because formatting information is stored in the AST node not in the child elements, see this:
   
    ![](03/May-21-2018%2012-47-23.gif)
* custom tech (?)
* no complex text layout support ![](03/Screen%20Shot%202018-05-21%20at%2012.51.04.png)
* complex selection and copy & paste behavior:
there is two level of selection block level and inline level
![](03/May-21-2018%2012-35-35.gif)
* editing commands ![](03/May-21-2018%2013-22-42.gif)
    * seems all commands will start with `\`, and when this char is entered, it create a command node immediately 
    * copy pasting `\` char will not create a command
* interoperability with other format: import/export as LaTeX

## Bear.app
* a structural editor that makes itself less (or more?) awkward by working like a Markdown plain text editor
	* that cause some annoying behavior sometimes:
		* ![](03/May-21-2018%2013-06-05.gif)
	* and has wired copy paste behavior (but you can imagine when they are helpful
		* ![](03/May-21-2018%2013-10-58.gif)
		* ![](03/May-21-2018%2013-12-36.gif)
* the implementation is based on macOS Cocoa text API
* selection: can select multiple ranges, and can use keyboard to navigate them
* data model: import/export from Markdown works ok, but not entirely the same
## web based WYSIWYG editors
seems all work the same to me…
