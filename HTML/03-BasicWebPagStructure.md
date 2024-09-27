# Basic Web Page Structure

## Entry point

Every website has a web page that is the entry point to the site and that is returned whenever a user navigates to the root URL for the site. The HTML for this entry point is stored in a file named '**index.html**'.

## Basic Tags

- **`<html></html>`** - Creates an HTML document
- **`<head></head>`** - Sets off the title & other information that isn’t displayed
- **`<body></body>`** - Sets off the visible portion of the document
- **`<title></title>`** - Puts the name of the document in the title bar; when bookmarking pages, this is what is bookmarked

## Basic Structure

Every web page (whether it is the entry point page or some other page) has the same basic structure, a document type element followed by an html element that contains a header element and a body element.

Exmaple:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body></body>
</html>
```
