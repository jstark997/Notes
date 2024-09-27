# Box Sizing

The `box-sizing` property in CSS defines how the total width and height of an element are calculated, including the padding and border.

There are two main values for `box-sizing`:

1. **`content-box` (default)**:

   - The width and height you set for the element only apply to the content.
   - Padding and borders are added _outside_ the content box, increasing the overall size of the element.
   - Example: If you set `width: 200px`, and the element has `padding: 10px` and `border: 5px`, the total width becomes `200px (content) + 10px (left padding) + 10px (right padding) + 5px (left border) + 5px (right border) = 230px`.

2. **`border-box`**:
   - The width and height include the content, padding, and borders.
   - This means the size you set is the total size of the element, so padding and borders are included within the specified dimensions.
   - Example: If you set `width: 200px`, the padding and border are included within that 200px, so the content size will shrink to make room for the padding and border.

Here's an example:

```css
.element {
  width: 200px;
  padding: 10px;
  border: 5px solid;
  box-sizing: border-box;
}
```

In this case, the total width remains 200px, with the content size adjusting to fit within the padding and border.

Using `box-sizing: border-box` is often more intuitive, as it avoids unexpected layout shifts caused by padding and borders increasing the element's size. It has become a common practice to apply it universally with this rule:

```css
* {
  box-sizing: border-box;
}
```

## Box Sizing: Border Box

![Box Sizing Border Box](/BoxSizingBorderBox.png "Box sizing with value border-box")
