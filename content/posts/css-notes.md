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

### Pseudo-elements

In terms of syntax, pseudo-elements use two colons instead of one (`::`), though some pseudo-elements also support single-colon syntax.

#### before and after

Two of the most common pseudo-elements are `::before` and `::after`.

```html
<style>
  p::before {
    content: '→ ';
    color: deeppink;
  }
  
  p::after {
    content: ' ←';
    color: deeppink;
  }
</style>
```



In general, **we probably shouldn't use these two pseudo-elements.** In a vanilla HTML/CSS world, it can be helpful to "bundle" content in with a CSS selector. In the era of components, however, we have better ways of bundling content.

There are also some accessibility concerns with `::before` and `::after`. Some screen readers will try to vocalize the `content`. Others will ignore them entirely. This inconsistency is problematic.

That said, if the effect is entirely decorative (eg. colorful shapes), I believe it's fine to create them with an empty `content` string:

```html
<style>
  p::before {
    content: '';
    display: block;
    width: 32px;
    height: 32px;
    border-radius: 50%;
    background-color: peachpuff;
    margin: 8px;
  }
</style>

<p>
  This paragraph has a decorative circle.
</p>
```



### Combinators

In CSS, we can differentiate between *children* and *descendants*. Think of a family tree: a child is only one level down from the parent. A descendant might be 1 level down (child), 2 levels down (grandchild), 3 levels down…

What if we only want to target children, and not deeper descendants? Using **`>`** will apply the style on the child only. 

```html
<style>
  li {
    margin-bottom: 8px;
  }
  
  .main-list > li {
    border: 2px dotted;
  }
</style>

<ul class="main-list">
  <li>Salt</li>
  <li>Pepper</li>
  <li>
    Fruits & Veg:
    <ul>
      <li>Apple</li>
    </ul>
  </li>
```







