# CSS Selectors

CSS selectors are used to select and style specific HTML elements. Below is a list of common types of CSS selectors with brief explanations.

**Note** - In general it is best practice is to avoid using the ID selector because it is too specific and combinator selectors (such as descendent selector) as they tend to tie styling too closely with the structure of the content which might change. For most use cases using a type or class selector is best. Also it is best practice to keep selectors as simple as possible.

## 1. Universal Selector (`*`)

- **Syntax**: `*`
- **Description**: Selects all elements on the page.
- **Example**:

  ```css
  * {
    margin: 0;
    padding: 0;
  }
  ```

## 2. Type Selector (Element Selector)

- **Syntax**: `element`
- **Description**: Selects all elements of a given type, such as `div`, `p`, `h1`, etc.
- **Example**:

  ```css
  p {
    color: blue;
  }
  ```

## 3. Class Selector

- **Syntax**: `.className`
- **Description**: Selects all elements that have the specified class attribute.
- **Example**:

  ```css
  .header {
    background-color: lightgray;
  }
  ```

## 4. ID Selector

- **Syntax**: `#idName`
- **Description**: Selects the element with the specific `id` attribute. IDs should be unique within a page.
- **Example**:

  ```css
  #main-title {
    font-size: 24px;
  }
  ```

## 5. Attribute Selector

- **Syntax**: `[attribute=value]`
- **Description**: Selects elements based on the presence or value of an attribute. Variations include:

  - `[attribute]` – Selects elements with the attribute.
  - `[attribute=value]` – Selects elements where the attribute equals a specific value.
  - `[attribute^=value]` – Selects elements where the attribute starts with a value.
  - `[attribute$=value]` – Selects elements where the attribute ends with a value.
  - `[attribute*=value]` – Selects elements where the attribute contains a value.
  - **Example**:

  ```css
  [type="text"] {
    border: 1px solid black;
  }
  ```

## 6. Descendant Selector

- **Syntax**: `ancestor descendant`
- **Description**: Selects all elements that are descendants of a specified ancestor element.
- **Example**:

  ```css
  div p {
    color: green;
  }
  ```

## 7. Child Selector

- **Syntax**: `parent > child`
- **Description**: Selects all direct children of a specified parent element.
- **Example**:

  ```css
  ul > li {
    list-style-type: none;
  }
  ```

## 8. Adjacent Sibling Selector

- **Syntax**: `element1 + element2`
- **Description**: Selects an element that is immediately preceded by the first specified element.
- **Example**:

  ```css
  h1 + p {
    margin-top: 0;
  }
  ```

## 9. General Sibling Selector

- **Syntax**: `element1 ~ element2`
- **Description**: Selects all elements that are siblings of the first specified element.
- **Example**:

  ```css
  h1 ~ p {
    color: darkgray;
  }
  ```

## 10. Pseudo-class Selector

- **Syntax**: `element:pseudo-class`
- **Description**: Selects elements based on their state. Common pseudo-classes include:

  - `:hover` – Selects an element when it is hovered over.
  - `:nth-child(n)` – Selects elements based on their position in a parent element.
  - **Example**:

  ```css
  a:hover {
    color: red;
  }
  ```

  **Note** - Pseudo-class selectors can be chained together.

  **Exaple**:

```css
nav a:link:last-child {
  margin-right: 0;
}
```

In the above the right margin is set to 0 for the last link in the navigation element.

## 11. Pseudo-element Selector

- **Syntax**: `element::pseudo-element`
- **Description**: Selects and styles specific parts of an element. Common pseudo-elements include:

  - `::before` – Inserts content before the element.
  - `::after` – Inserts content after the element.
  - `::first-letter` – Selects the first letter of the element.
  - **Example**:

  ```css
  p::first-letter {
    font-size: 2em;
    font-weight: bold;
  }
  ```

## 12. Grouping Selector

- **Syntax**: `selector1, selector2`
- **Description**: Groups multiple selectors together so they share the same style rules.
- **Example**:

  ```css
  h1,
  h2,
  h3 {
    font-family: Arial, sans-serif;
  }
  ```

## 13. Combinator Selectors

- **Syntax**: Various
- **Description**: These selectors combine multiple elements in various ways. Examples include:

  - Descendant Selector (` `): Selects all descendants.
  - Child Selector (`>`): Selects direct children.
  - Adjacent Sibling Selector (`+`): Selects adjacent siblings.
  - General Sibling Selector (`~`): Selects all siblings.

## 14. :not() Selector

- **Syntax**: `:not(selector)`
- **Description**: Excludes elements that match the selector within `:not()`.
- **Example**:

  ```css
  p:not(.highlight) {
    color: gray;
  }
  ```

---

This list covers the most common types of CSS selectors used for targeting HTML elements.
