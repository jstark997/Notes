# CSS Rule

A CSS rule describes the style for a specific HTML element or set of elements.

The HTML element or set of elements that a CSS rule pertains to is indicated by the '**selector**' which is at the beginning of the rule defintion.

After the selector is a set of curly braces which designates the '**declaration block**' of the rule.

Within the declaration block are one or more '**declarations**' or '**styles**' which start with a property name followed by a colon and then the value(s) for the property and ending with a semi-colon.

Example:

```css
h1 {
  color: blue;
  text-align: center;
  font-size: 20px;
}
```

In the above example, 'h1' is the selector specifying that this rule pertains to all h1 elements. Within the curly braces are the styles setting the color, text position and font-size for the h1 elements.
