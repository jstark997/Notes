# Dimensions

In CSS the dimensions of the content of an element can be specified with the **height** and **width** properties.

**Note** - The total visible height and width of an element is **not** specified by the height and width properties, because of the padding and border which are still within the element. Therefore the total height of an element is the width of the border top and bottom plus the padding top and bottom plus the height of the content. And the total width of an element is the width of the border left and right plus the width of the padding left and right plus the width of the conent.

**Note** - Be careful setting a fixed height for an element that is a parent element as if the children of this element change so as to not fit within the fixed height those children will be rendered as spilling outside of the parent element.

## Auto

The value **auto** for height and width means that the browser will automatically calculate those values so that the content of the element is fully displayed. This value is often the default value of many elements.

Setting a height or width property to auto in CSS is usually not necessary unless those properties have been set directly on the HTML element and one of those properties is set in CSS. The property that is set in CSS will override the property set in HTML which for some elements, such as images, can distort the content. By setting the other property to 'auto' the browser will automatically calculate the value that is necessary to properly display the content (in the case of images this means preserving the aspect ratio).

Example:

```html
<img
  src="img/post-img.jpg"
  alt="HTML code on a screen"
  width="500"
  height="200"
  class="post-img"
/>
```

```css
.post-img {
    width: 800px
    height: auto
}
```

In the above example the image element has both height and width set in HTML, but the CSS rule for the element sets the width to 800 pixels. Without also setting the height value to auto in CSS the image would be distorted as it would be stretched to fit the set height of 200 and the aspect ratio would not be preserved.

## Percentage Values

Instead of pixels (which is an absolute size value) height and width can be specified in terms of the percentage of the enclosing element that is taken to display the element.

Example:

```css
.post-img {
    width: 100%
    height: auto
}
```

In the above example the width is set to 100% which means that the image will take up 100% of the width of the enclosing element. This will be true even if the display changes size. Therefore, using percentage values is one way of creating responsive designs.
