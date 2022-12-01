---
title: "CSS for JS Notes (1)"
date: 2022-11-30T23:05:55-08:00
draft: true
toc: false
images:
tags:
  - Notes
  - CSS
categories:
  - Front-End
---
Course: [CSS for JS](https://css-for-js.dev/)
## Module 0 - Fundamentals
## Media Queries

```css
/* CSS */
@media (condition) {
  /* Some CSS that'll run if the condition is met. */
}
```
#### Hiding content
It's common to use media queries to have alternative interfaces depending on the screen size.
![demo 1](/2.png)

`display`: none is a declaration that removes an element from the rendering process; it's as if it doesn't exist.

By default, we'll hide any elements with the `large-screens` class.

If our window is at least 300px wide, however, we apply special overrides. This includes showing `large-screens` elements, and hiding `small-screens` elements.

This trick is often used for navigation. Desktop users see a list of links, whereas mobile users see a hamburger icon.

## Selectors

### Pseudo-classes

Pseudo-classes let us apply a chunk of CSS based on an element's current state. In this case, we're adding a blue text color when the element is hovered.

```css
/* Any button over which the user's pointer is hovering */
button:hover {
  color: blue;
}
```

#### focus

The `:focus` pseudo-class allows us to apply styles exclusively when an interactive element has focus:

![demo 2](/3.png)

> Focus styles are primarily useful for folks who don't use a "pointer-style" input device (like a mouse, a trackpad, or a finger on a touchscreen). The focus styles show you where you are on the page, which element is selected.

#### checked

The `:checked` pseudo-class only applies to checkboxes and radio buttons that are "filled in". You can apply additional styles to indicate that the input is activated:

![demo 3](/4.png)

#### first/last-child; first/last-of-type

```css
li:last-child {
  color: red;
}
```

![demo 4](/5.png)

`:first-of-type` depends on the *type* of the HTML tag. For example, let's suppose we have the following setup:

```html
<style>
  p:first-child {
    color: red;
  }
</style>

<section>
  <h1>Hello world!</h1>
  <p>This is a paragraph!</p>
  <p>This is another paragraph!</p>
</section>
```

The first child within the parent `<section>` tag is an `<h1>`. Our `p:first-child` is looking for situations where a *paragraph* is the first child within a parent container. It doesn't work in this case.

But, if we switch the selector to `p:first-of-type`, it *does* work:

```html
<style>
  p:first-of-type {
    color: red;
  }
</style>

<section>
  <h1>Hello world!</h1>
  <p>This is a paragraph!</p>
  <p>This is another paragraph!</p>
</section>
```

The `:first-of-type` pseudo-class ignores any siblings that aren't of the same type. In this case, `p:first-of-type` is going to select the first *paragraph* within a container, regardless of whether or not it's the first *child*.











