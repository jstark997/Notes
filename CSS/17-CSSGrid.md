# CSS Grid

![What Is CSS Grid](/WhatIsCSSGrid.png "What is CSS grid")

## Structure And Terminology

### Basic Structure

![Basic CSS Grid Structure](/GridStructrue1.png "Basic CSS grid structure")

### Detailed Structure

![Detailed CSS Grid Structure](/GridStructure2.png "Detailed CSS grid structure")

## Grid Container And Grid Item Properties

![Grid Container And Grid Item Properties](/GridProperties.png "Grid container and grid item properties")

## The fr And auto Values

In CSS Grid, `fr` (fractional unit) and `auto` are two types of values used with the `grid-template-columns` and `grid-template-rows` properties to define the sizes of columns and rows. Here's a breakdown of how each one works:

### 1. **`fr` (fractional unit)**

The `fr` unit represents a fraction of the available space in the grid container. It allows you to create flexible layouts where columns (or rows) share the space according to the specified proportions.

For example:

```css
grid-template-columns: 1fr 2fr;
```

In this case:

- The first column will take up 1 fraction of the available space.
- The second column will take up 2 fractions of the available space.

This means that if the grid container has a width of 300px, the first column would take 100px (1/3 of the available space) and the second column would take 200px (2/3 of the available space).

Key points about `fr`:

- It only considers available space. Space used by other units like fixed pixel sizes or content-sized (`auto`) columns is subtracted first, and the remaining space is divided into the `fr` units.

### 2. **`auto`**

The `auto` value allows the size of the column (or row) to be determined by the size of its content. It grows or shrinks based on the content inside the grid cell.

For example:

```css
grid-template-columns: auto 1fr;
```

In this case:

- The first column will automatically size itself based on the largest content inside that column. If the content is 100px wide, the column will be 100px wide.
- The second column will take up the remaining space using the `1fr` unit.

Key points about `auto`:

- The size of the column or row can vary based on the content.
- It is often used when you want the column or row to "shrink-wrap" around the content, and not take up more space than necessary.

### Example with both `fr` and `auto`:

```css
grid-template-columns: auto 1fr 2fr;
```

- The first column will adjust to the width of its content (`auto`).
- The second column will take 1 fraction of the remaining space (`1fr`).
- The third column will take 2 fractions of the remaining space (`2fr`).

In summary:

- **`fr`** units divide up the remaining space proportionally.
- **`auto`** lets the column or row size itself based on its content.

## Repeat Function

The `repeat` function takes 2 argument: the number of columns or rows to create and the size each column or row shold be.

For example:

```css
grid-template-columns: repeat(4, 1fr);
```

The above creates within a grid 4 columns of equal size.
