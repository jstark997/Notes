# Positioning

Modes:

- Normal Flow (position: relative)
- Absolute (position: absolute)

Normal flow is the default.

![Positioning Modes](/PositioningModes.png "Positioning modes")

## Absolute Positioning

Absolute positioning is set by the properties top, bottom, left and right. By default those properties set the position relative to the viewport (the visible part of the display when the values are read by the browser). However, that means if the viewport changes the position of the element will not change but will remain where it originally was positioned. Therefore it is best practice to position the element relative to a parent element, such as the body. However, that parent element must have position set to relative for this to work. Also, the element will be positioned relative to the first parent element that has position set to relative.

![Absolute Positioning](/AbsolutePositioning.png "Absolute positioning")

## Negative Pixel Values

Pixel values used for properties such as top, bottom, left and right can be positive or negative. If the value is negative it will cause the element to be positioned in the opposite direction of the property. For example, setting the right property of an absolute positioned element will move the element 25px from the right of the parent element (towards the left side of the parent), However, setting the righ property to -25px will move the element -25 pixels from the right edge of the parent (toward to the right).
