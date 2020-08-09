# Chapter 3 - How browsers work
This chapter covers:
- Web page rendering
- The browser security model
- Attacks on the browser

## Web Page Rendering
The part of the browser that transforms HTML into something that can be displayed by the OS is known
as the **rendering pipeline**.

Early on this pipeline was simple, as there was very little in terms of stying. HTML is a markup
language, so it's more about how a (mostly text) document is layed out, and early websites were
mostly just HTML.

Now there's CSS to control how to display each element of the HTML, which complicates things.

### The Rendering Pipeline: An Overview
At a high level:
1. Browser parses the body of an HTTP response into a Document Object Model (DOM), which represents
   the structure of the page.
   - The *whole* of the page has to be parsed, as the order of the tags does not determine their
     location on the page
2. Before rendering, styling is determined for each element in the DOM.
3. Screen is rendered for each frame

Also, the browser executes JavaScript (either on page load or triggered by the client), which can
affect the DOM and/or the styling, changing the appearance of the page.

### The Document Object Model
This is a data structure (a tree) which describes the HTML document. Nodes within this structure are
known as DOM nodes. These nodes are roughly equivalent to an HTML tag in the document.

If an HTML tag references an external resource (e.g. `<image>`, `<script>`, `<style>`), those
resources are fetched during the DOM parsing step with another HTTP request. Modern browsers do this
alongside page rendering to make loading more responsive.

The browsers are very lenient about malformed HTML when building the DOM tree (e.g. missing tags).

### Styling information
After the DOM tree is constructed, it must determine how to lay out visible elements of the tree and
how to style them. Styling information is usually seperated out into CSS files to keep the HTML
semantic, which (amongst other things) improves accessibility. CSS is imported using `<style>` tags.

Every stylesheet has *selectors*, which specify the HTML tag they affect and then define the styling
that should be applied to that tag.

## JavaScript
JavaScript is found within `<script>` tags - either defined inline, or (more usually) referenced as
an external resource and fetched on page load.

The default behaviour is to immediately execute JavaScript in when the corresponding `<script>` tag
is parsed into a DOM node. This is a problem if the rest of the HTML document hasn't been parsed
into the tree, as the JavaScript may attempt to interact with elements in the tree that are not
there yet. To fix this, `<script>` tags usually have a `defer` attribute, which means the JavaScript
is only executed once the DOM has been constructed.
