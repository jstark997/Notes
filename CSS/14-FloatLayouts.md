# Float Layouts

The `float` property in CSS is used to control how elements are positioned within a container, allowing text and inline elements to wrap around them. It’s commonly used to create layouts, especially for things like placing images alongside text. Here's how it works:

- **Possible values**:

  - `left`: The element floats to the left side of its container, and other content flows around it on the right.
  - `right`: The element floats to the right side of its container, and other content flows around it on the left.
  - `none`: The element does not float, and is displayed in the normal document flow (this is the default).
  - `inherit`: The element inherits the float value from its parent.

- **Common use cases**:

  - Floating an image to one side of a paragraph so the text wraps around it.
  - Creating multi-column layouts before the widespread use of Flexbox and Grid.

- **Important considerations**:
  - Elements after a floated element will wrap around it unless explicitly cleared using the `clear` property (`clear: left`, `clear: right`, or `clear: both`).
  - A floated element is removed from the normal document flow, which can cause issues like its parent container collapsing if not handled properly (e.g., by using a clearfix or adding `overflow: hidden` to the parent).

Though `float` was historically used to create layouts, modern CSS layout techniques like Flexbox and Grid have largely replaced it for that purpose. However, it's still occasionally useful for simpler tasks like wrapping text around images.

## Comparing Positioning Modes With Float

![Absolute Positioning Vs Floats](/AbsolutePositoningVsFloats.png "Absolute positioning versus floats")

## Clearfix Hack

Because floated elements within a parent can cause the parent to collapse one method of fixing this problem was to add an empty div within the parent element and using CSS to select that div set the clear property on that div to both, which would uncollapse the parent element.

However, a better method for solving the some problem is the 'clearfix hack' shown below:

```css
.clearfix::after {
  clear: both;
  content: "";
  display: block;
}
```

The above will insert a pseudo-element after the content within the parent and by setting the clear property to 'both' will clear any floats that are either on the left or right preventing the parent from collapsing. However, the clear property only works on block level elements and since inserted pseudo-elements are inline the display property must be set to 'block'.
