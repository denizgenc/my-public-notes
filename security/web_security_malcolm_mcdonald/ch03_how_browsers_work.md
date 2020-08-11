# Chapter 3 - How browsers work
This chapter covers:
- Web page rendering
- JavaScript and the browser security model
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

The default behaviour is to immediately execute JavaScript when the corresponding `<script>` tag is
parsed into a DOM node. This is a problem if the rest of the HTML document hasn't been parsed into
the tree, as the JavaScript may attempt to interact with elements in the tree that are not there
yet. To fix this, `<script>` tags usually have a `defer` attribute, which means the JavaScript is
only executed once all the HTML is parsed into the DOM.

JavaScript is an enticing target for attackers as it opens up the possibility of remote code
execution in lots of different ways. To reduce the avenues in which an attacker can use JavaScript,
we have the **browser security model**.

The security model executes JavaScript in a sandbox, where it cannot do the following:
- Start new processes
- Access other processes
- Access arbitrary portions of the system memory (can only access memory from within its own
  sandbox)
- Read and write from the disk. Sites can store data locally, but there's no interface to the actual
  filesystem.
- Access the OS's network layer
- Call OS functions

So what *can* JavaScript do?
- Read/write to the DOM
- Create ("register") event listeners to respond to user actions on the page
- Make HTTP requests
- Open new web pages or refresh the URL, but only on a user action
- Write to the browser history, and go backwards and forwards in the history
- Ask for the user's location, for permission to send desktop notifications, for mic access etc.

These restrictions don't stop cross-site scripting (XSS) attacks, which can read credit card
details, credentials, etc.  Even a tiny amount of code can be dangerous - because JavaScript can
write to the DOM, a one liner can pull in more and more `<script>` tags. Protecting a site against
cross-site scripting is covered in chapter 7.

## Everything else the browser does
Browsers aren't just rendering pipelines and JavaScript interpreters:
- Communicate with the OS to resolve and cache DNS queries
- Verify security certificates
- Perform TLS handshakes and encrypt HTTP
- Store and transmit cookies
