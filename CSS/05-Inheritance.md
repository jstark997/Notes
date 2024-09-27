# Inheritance

HTML elements will inherit any applicable CSS styles from a parent element. However, child elements can override any inherited styles through CSS rules that apply to them.

Exmaple:

![CSS Inheritance Example](/CSSInheritanceExample.png "Example of CSS inheritance")

## Universal Selector

The universal selector ('\*') will apply styles to all the elements in a web page. However, it has the lowest priority of any selector and can be overriden by any rule. It is important to understand that this feature of the universal selector is not inheritance as it is not a parent element. In contrast the body element is a parent to all other content elements such as headings, paragraphs, images, etc.
