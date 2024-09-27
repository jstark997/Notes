# Margins And Padding

Padding is the space between the content of an element and its border. Margin is the space between elements.

## Padding Properties

- padding-top
- padding-bottom
- padding-left
- padding-right
- padding

The padding property takes either 1, 2 or 4 values. If 1 value is passed then it is set as the value for the top, bottom, left and right padding. If 2 values are passed the first value is both the top and bottom padding and the second value is both the left and right padding. If 4 values are passed they are in the order of top, bottom, left and right values.

**Note** - When setting a padding property to 0 no units are specified.

## Margin Properties

- margin-top
- margin-bottom
- margin-left
- margin-right
- margin

The margin property takes either 1, 2 or 4 values. If 1 value is passed then it is set as the value for the top, bottom, left and right margin. If 2 values are passed the first value is both the top and bottom margin and the second value is both the left and right margin. If 4 values are passed they are in the order of top, bottom, left and right values.

**Note** - When setting a margin property to 0 no units are specified.

**Note** - Best practice is the use only margin-bottom when setting vertical space between elements.

## Using Universal Selector To Reset Margins And Padding

Often the universal selector is defined at the very beginnin with margin and padding set to 0 in order to reset the default element values. This way element can be defined with specific margin and padding values.

Example:

```css
* {
  margin: 0;
  padding: 0;
}
```

## Margins and Lists

Ordered and unordered lists need enough of a margin on the left in order to display numbers or dots. If the universal selector is used to reset the default margins to 0, then a CSS rule for ordered and unordered lists will need to be created to set the left margin of these elements in order for them to be displayed properly.

## Collapsing Margins

When elements have overlapping margins, those margins will not be additive. Instead they will be collapsed into a single margin with the value of the element with the larger margin.

Example:

```html
<p>Some text.</p>
<h3>A Heading</h3>
```

```css
h3 {
  font-size: 30px;
  margin-bottom: 20px;
  margin-top: 40px;
}

p {
  font-size: 22px;
  line-height: 1.5;
  margin-bottom: 15px;
}
```

In the above example the paragraph element and the heading element are right next to each other. The paragraph element has a bottom margin of 15 pixels and the heading element has a top marigin of 40 pixels. These overlapping margins will be collapsed by the browser to 40 pixels between the elements because the heading element has the larger value for the top margin.
