# CSS Grid Item Placement

In CSS Grid Layout, the `grid-column` and `grid-row` properties are used to position grid items within the grid, allowing you to specify where an item should be placed both horizontally and vertically. Additionally, negative values can be used for the start or end values to count from the last grid line.

### `grid-column`

- **Purpose**: Defines where a grid item should start and how many columns it should span.
- **Syntax**: `grid-column: start / end;`
  - `start`: The starting grid line (can be positive or negative).
  - `end`: The ending grid line (also can be positive or negative).

Positive numbers count from the first grid line (left to right), while negative numbers count from the last grid line (right to left).

**Example using positive values:**

```css
.item {
  grid-column: 2 / 4;
}
```

In this example, the grid item starts at the second column line and spans up to the fourth column line, occupying two columns.

**Example using negative values:**

```css
.item {
  grid-column: -3 / -1;
}
```

Here, the item starts at the third column line from the end (counting from the right), and ends at the first column line from the right, spanning two columns.

You can also use the `span` keyword to control how many columns the item should span:

```css
.item {
  grid-column: 2 / span 2;
}
```

This places the item starting at the second grid line and spans two columns.

### `grid-row`

- **Purpose**: Defines where a grid item should start and how many rows it should span.
- **Syntax**: `grid-row: start / end;`
  - `start`: The starting grid line (can be positive or negative).
  - `end`: The ending grid line (can also be positive or negative).

Positive values count from the first row line (top to bottom), while negative values count from the last row line (bottom to top).

**Example using positive values:**

```css
.item {
  grid-row: 1 / 3;
}
```

In this case, the item starts at the first row line and spans up to the third row line, occupying two rows.

**Example using negative values:**

```css
.item {
  grid-row: -4 / -2;
}
```

This positions the item starting from the fourth row line from the bottom and ends at the second row line from the bottom, spanning two rows.

Like with `grid-column`, you can use the `span` keyword:

```css
.item {
  grid-row: 1 / span 2;
}
```

This starts the item at the first row and spans it across two rows.

### Key Points

- `grid-column: start / end;` controls the horizontal positioning and span of an item. Negative values count from the end of the grid.
- `grid-row: start / end;` controls the vertical positioning and span of an item. Negative values count from the bottom of the grid.
- The `span` keyword can be used to define how many columns or rows the item should span from the start line.
