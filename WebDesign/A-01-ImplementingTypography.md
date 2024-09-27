# Implementing Typography

## Linking To External Fonts

To be able to use fonts in a web page they need to be linked to that web page. The `link` tag can be used for this purpose.

**Example**:

```html
<head>
  <link rel="preconnect" href="https://fonts.gstatic.com" />
  <link
    rel="stylesheet"
    href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap"
  />
  <link rel="stylesheet" href="style.css" />
</head>
```

In the above a link to the Google Fonts Inter font for font weights 400, 500 and 700 is specified.

**Note** - The links for fonts must come before the link to the CSS file, in this case 'style.css', that the web page will use so that the fonts are loaded and available before the CSS file is loaded.

## Setting A Preferred Font In CSS

To set a prefered font in CSS use the `font-family` property and set it to the name of the preferred font.

**Example**:

```css
body {
  font-family: "Inter", sans-serif;
}
```

In the above example the font-family property is set to use the 'Inter' font if available, otherwise the default sans-serif font will be used. This property is set in the rule for the body element which means all child elements will inherit that rule and will therefore use that font.

## Setting Text Properites

The CSS properites `font-size`, `line-height` and `letter-spacing` can be set to affect the appearance of lines of text for different elements.

**Example**:

```css
h1 {
  font-size: 44px;
  line-height: 1.1;
  letter-spacing: -1px;
}
```

The above values are appropriate for a level 1 heading, but other text elements would have different values depending on the readability and aesthetic requirements.
