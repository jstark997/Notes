# CSS Grid Alignment

In CSS Grid, these alignment and justification properties help position grid items and the grid container's content along both the horizontal (main) and vertical (cross) axes. Here’s a breakdown of how each property works:

### 1. **`align-content`** (Vertical Alignment of the Grid Container’s Content)

- This property aligns the grid content (the entire grid) vertically within the grid container when there is extra space on the cross axis (typically the vertical axis).
- It affects the space between rows of the grid and can take values like `start`, `end`, `center`, `space-between`, `space-around`, `space-evenly`, and `stretch`.

**Example**:

```css
.grid-container {
  align-content: center;
}
```

This centers the entire grid vertically if the container has extra space.

### 2. **`justify-content`** (Horizontal Alignment of the Grid Container’s Content)

- Similar to `align-content`, but it controls the horizontal alignment of the grid within the container when there is extra space on the main axis (typically the horizontal axis).
- Values: `start`, `end`, `center`, `space-between`, `space-around`, `space-evenly`, `stretch`.

**Example**:

```css
.grid-container {
  justify-content: space-between;
}
```

This distributes extra space between the grid columns.

### 3. **`align-items`** (Vertical Alignment of Individual Grid Items)

- Aligns the grid items within their grid areas along the cross axis (vertical).
- It determines how grid items are placed within their row if there is extra space within the item’s cell.
- Values: `start`, `end`, `center`, `stretch` (default).

**Example**:

```css
.grid-container {
  align-items: center;
}
```

This vertically centers all grid items in their respective rows.

### 4. **`justify-items`** (Horizontal Alignment of Individual Grid Items)

- Aligns grid items within their grid areas along the main axis (horizontal).
- Determines how items are placed within their column if there is extra space within the item’s cell.
- Values: `start`, `end`, `center`, `stretch` (default).

**Example**:

```css
.grid-container {
  justify-items: start;
}
```

This aligns all items to the left edge of their cells.

### 5. **`align-self`** (Vertical Alignment of a Specific Grid Item)

- This property allows individual grid items to override the `align-items` property.
- It controls the vertical alignment of a specific grid item within its grid area.
- Values: `start`, `end`, `center`, `stretch`.

**Example**:

```css
.grid-item {
  align-self: end;
}
```

This aligns one specific item at the bottom of its cell.

### 6. **`justify-self`** (Horizontal Alignment of a Specific Grid Item)

- Allows individual grid items to override the `justify-items` property.
- It controls the horizontal alignment of a specific grid item within its grid area.
- Values: `start`, `end`, `center`, `stretch`.

**Example**:

```css
.grid-item {
  justify-self: center;
}
```

This centers one specific item horizontally within its cell.

### Summary:

- **`align-content`**: Vertical alignment of the entire grid.
- **`justify-content`**: Horizontal alignment of the entire grid.
- **`align-items`**: Vertical alignment of all items in their grid areas.
- **`justify-items`**: Horizontal alignment of all items in their grid areas.
- **`align-self`**: Vertical alignment of a specific grid item.
- **`justify-self`**: Horizontal alignment of a specific grid item.

These properties offer fine-grained control over grid layouts, allowing for flexible and responsive designs.
