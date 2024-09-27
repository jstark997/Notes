# Flexbox

![What Is Flebox](/WhatIsFlexbox.png "What is flexbox")

## Structure And Terminology

![Flexbox Terminology](/FlexboxTerminology.png "Flexbox terminology")

## Flex Container and Flex Item Properties

![Flexbox Properties](/FlexProperties.png "Flex container and item properties")

The `order` property by default is 0 for every flex item. To move an item to the beginning specify a number smaller than 0 (-1 for example). To move an item to the end specify a value larger than 0 (1 for example). To move another item either in front of or behind the previously moved items, specify a number less then one for the beginning item or greater than the one for the last item.

The `gap` property will create space between elements without creating a margin.

The `flex-basis` property is more of a recommendation to the browser as to what the width of the item should be. If the item is much smaller or much bigger than the size set as the flex-basis the browser will shrink or expand the item to better fit the content.

The `flex-grow` property controls how much an item should grow relative to the others when there's extra space in the container. Setting this property to 1 for all the items will cause them to take up the some amount of space within the container. Setting this property for an item to a value larger than that of the other items will cause that item to take proportionally more space within the container than the other items.

The `flex-shrink` property defines how much an item should shrink if there's not enough space in the container.
