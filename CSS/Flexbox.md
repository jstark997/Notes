# Flexbox

**Flexbox**, or the **Flexible Box Model**. is a one dimensional layout model (that is part of native CSS) that provides a method to offer space distribution and alignment capabilities.

Another option for positioning and alignment is **CSS Grid**.

## Flex Container And Items

The **flex container** is the element that holds **flex items**, which are the direct children of flex containers. A container is created when, in the CSS file, 'display: flex' is set as a property on the class for the container. Flex items can themselves be flex containers.

Example:

```css
.flex-container {
  display: flex;
}
```

```html
<div class="flex-container">
  <div>flex item 1</div>
  <div>flex item 2</div>
  <img src="pic.png" alt="also a flex item" />
</div>
```

In the above the outer div has a class of 'flex-container' upon which 'display: flex' has been set as a property (in the CSS file). So all the inner divs are flex items of this flex container. The flex items will be dispalyed in a row by default.

## Flex Container Properties

- flex-direction
- flex-wrap
- flex-flow
- justify-content (always on the main axis)
- align-itmes (always on the creoss axis)
- align-content

## Flex Item Properties

- order
- flex-grow
- flex-shrink
- flex-basis
- align-self

## Axes

There are 2 axes:

- Main axis - the axis that is parallel with the flex-direction
- Cross axis - the axis that is perpendicular to the flex-direction

If 'flex-direction: row' (the default), then the main axis will be horizontal and the cross axis will be vertical.

If 'flex-direction: col', then the main axis will be vertical and the cross axis will be horizontal.

## Flex Property

The property 'flex' takes 3 possible arguments:

1. grow
2. shrink
3. basis

Example:

```css
.item {
  flex: 1 0 100px;
  display: flex;
}
```
