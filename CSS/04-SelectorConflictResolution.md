# Declaration Conflict Resolution

It is possible to write CSS rules with selectors that match the same HTML element but with declarations for the same property with different values. These conflicts arise because an element can be matched by many different possible selectors (element type, class, ID, etc.). The browser will apply all the declarations defined in every rule that matches an element.

In the case where matching rules specify declarations for the same property with different values there are rules for resolving the conflict and deciding which declaration will apply. Each type of selector has a priority. Declarations within rules with selectors of a higher priority are applied over declarations within rules with selectors of a lower priority.

Below is a list the selector priorities from highest to lowest.

1. Declarations marked with '!important'
2. Inline style (style attribute in the HTML)
3. ID selector
4. Class (.) or pseudo-class (:) selector
5. Element selector (h1, p, div, li, etc.)
6. Universal selector (\*)

In the case where multiple selectors of the same priority match an element, then the selector listed last in the code will have the higher priority.

Example:

```css
.author {
  font-style: italic;
  font-size: 18px;
}

#author-text {
  font-size: 20px;
}

p,
li {
  font-family: sans-serif;
  color: #444444;
  font-size: 22px;
}
```

```html
<p id="author-text" class="author">
  Posted by Laura Jones on Monday, June 21st 2027
</p>
```

In the above example all of the CSS rules apply to the HTML paragraph element. Therefore all of the declarations will be applied, for font-style, font-family, color and font-size. However, each rule defines a different value for font-size. But the rule with the ID selector has the highest priority, therefore the font-size value from that rule it the one that will be applied by the browser.
