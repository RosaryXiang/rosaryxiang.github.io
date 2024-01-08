---
layout: default
title: Introducing Methods
parent: CSS
nav_order: 1
---

#### _January 7th, 2023_

# **Methods of Introducing CSS**

## **Internal Style**

Add declarations inside the **`<head>`**:

```html
htmlCopy code
<head>
  <style>
    h1 {
      color: blue;
      font-size: 12px;
    }
    p,
    h3 {
      color: red;
    }
  </style>
</head>
```

**`h1`** is the selector, and the contents within **`{}`** are the styles.

For multiple web pages, this needs to be copied multiple times in the header.

## **Inline Style**

```html
htmlCopy code
<body>
  <p style="color: red; font-size: 30px;">I am an inline style</p>
</body>
```

Not conducive to maintenance.

## **External Style**

public.css:

```css
cssCopy code p {
  color: red;
  font-size: 20px;
}
```

```html
htmlCopy code
<head>
  <link rel="stylesheet" href="./public.css" />
</head>
```
