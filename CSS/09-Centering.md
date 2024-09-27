# Centering

To center a web page within the display of the browser there needs to be an element that contains everything in body of the web page.

Typically this element is a division element designated by the 'div' tag.

Then using CSS the division element can be given a specific width and a margin left and right set to auto. The width of the web page will remain the same as the display changes size but the margins will grow or shrink keeping the content centered.

Example:

```html
<body>
  <div class="container">...</div>
</body>
```

```css
.container {
  width: 700px;
  /* margin-left: auto;
    margin-right: auto; */
  margin: 0 auto;
}
```

In the above any elements within the divison element will be centered within the browser display. In the CSS the margin-left and margin-right properties, which are one way of setting the appropriate margin, are commented out in favor of the using the margin property with 2 values (top and bottom, and left and right).
