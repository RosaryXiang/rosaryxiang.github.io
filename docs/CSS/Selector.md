---
layout: default
title: Selectors
parent: CSS
nav_order: 2
---

#### _January 7th, 2023_

# **Selectors**

## **Global Selector**

```css
cssCopy code * {
  color: red;
  font-size: 20px;
}
```

Lowest priority, generally used for style initialization.

## **Element Selector**

Tag selector.

## **Class Selector**

```css
cssCopy code .classname {
  color: blue;
}

.size {
  size: 20px;
}
```

```html
htmlCopy code
<p class="classname size">hello!</p>
```

Class selectors can be used by multiple tags.

A single tag can use multiple class selectors, separated by spaces.

## **ID Selector**

```html
htmlCopy code
<head>
  <style>
    #idname {
      color: blue;
    }
  </style>
</head>

<body>
  <p id="idname">Ha ha</p>
</body>
```

Can only be used once.

## **Selector Priority**

**Inline style 1000 > ID selector 100 > Class selector 10 > Element selector 1**

```html
htmlCopy code
<head>
  <style>
    .t1 {
      color: red;
    }
    .t2 {
      color: green;
    }
  </style>
</head>

<body>
  <h1 class="t1 t2">which one?</h1>
</body>
```

**`t1`** **`t2`** execute in order, **`t2`** overrides **`t1`**.

## **Descendant Selector**

```css
cssCopy code ul li {
  color: red;
}
```

## **Child Selector**

```css
cssCopy code ul > li {
  color: red;
}
```

## **Adjacent Sibling Selector**

```css
cssCopy code h3 + p {
  color: red;
}
```

## **General Sibling Selector**

```css
cssCopy code ul ~ li {
  color: red;
}
```
