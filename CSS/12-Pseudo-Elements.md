# Pseudo-Elements

Pseudo-elements in CSS are used to style specific parts of an element without needing to add additional HTML. They allow you to target and style certain parts or aspects of elements that aren’t directly accessible via traditional selectors. Pseudo-elements are typically preceded by a double colon (`::`) to distinguish them from pseudo-classes, though some older pseudo-elements still support the single colon syntax (`:`).

Common pseudo-elements include:

1. **`::before`** – Inserts content before the content of the selected element.
2. **`::after`** – Inserts content after the content of the selected element.
3. **`::first-letter`** – Targets the first letter of the selected element.
4. **`::first-line`** – Targets the first line of the selected element's content.
5. **`::selection`** – Styles the portion of an element that the user selects (e.g., text highlight color).

For example:

```css
p::first-line {
  font-weight: bold;
  color: blue;
}

p::before {
  content: "Note: ";
  font-weight: bold;
}
```

In this case:

- The first line of any paragraph will be bold and blue.
- The `::before` pseudo-element will insert "Note: " before the content of every paragraph.

Pseudo-elements are very useful for applying styles to parts of an element or adding decorative content without modifying the HTML structure itself.

Pseudo-elements inserted with `::before` and `::after` are inline elements and must have the content property set to some value (with a blank string, '', being acceptable).
