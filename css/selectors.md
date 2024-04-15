# Selectors

## Basic Selectors

### Universal

Selects all elements

```css
* {
		...
}
```

### Type

Select elements of that type

```css
div {
    ...
}
```

### Class

Select elements with that class

```css
.class {
    ...
}
```

### Id

Select elements with that id

```css
#id {
    ...
}
```

## Combination Selectors

### Descendant&#x20;

Select elements that are descendants of the first element

```css
div a {
    ...
}
```

### Direct Child

Select elements that are direct children of the first element

```css
div > a {
    ...
}
```

### General Sibling

Select elements that are siblings of the first element and come after the first element

```css
div ~ a {
    ...
}
```

### Adjacent Sibling

Select elements that are siblings of the first element and come directly after the first element

```css
div + a {
    ...
}
```

### Or

Select elements that match any selector in the list

```css
div, a {
    ...
}
```

### And

Select elements that match all the selector combinations

```css
div.c {
    ...
}
```

## Attribute Selectors

### Has

Select elements that have that attribute

```css
[a] {
    ...
}
```

### Exact

Select elements that have that attribute with exactly that value

```css
[a = "1"] {
    ...
}
```

### Begins With&#x20;

Select elements that have that attribute which start with that value

```css
[a^= "1"] {
    ...
}
```

### Ends With&#x20;

Select elements that have that attribute which end with that value

```css
[a$= "1"] {
    ...
}
```

### Substing

Select elements that have that attribute which contain that value anywhere

```css
[a*= "1"] {
    ...
}
```

## Pseudo Element Selector

### Before

Creates an empty element directly before the children of selected element

```css
div::before {
    ...
}
```

### After

Creates an empty element directly after the children of selected element

```css
div::after {
    ...
}
```

## Pseudo Class (State) Selector

### Hover

Select elements that are hovered by the mouse

```css
div:hover {
    ...
}
```

### Focus

Select elements that are focused.

```css
div:focus {
    ...
}
```

### Active

Select elements that are activated.

```css
div:focus {
    ...
}
```

### Required

Select elements that are required

```css
div:required {
    ...
}
```

### Checked

Select elements that are checked

```css
div:checked {
    ...
}
```

### Disabled

Select elements that are disabled

```css
div:disabled {
    ...
}
```

## Pseudo Class (Position/Other) Selector

### First Child

Select elements that are the first child inside a container

```css
div:first-child {
    ...
}
```

### Last Child

Select elements that are the last child inside a container

```css
div:last-child {
    ...
}
```

### Nth Child

Select elements that are the nth child inside a container based on the formula

```css
div:nth-child(n) {
    ...
}
```

### Nth Last Child

Select elements that are the nth child inside a container based on the formula counting from the end

```css
div:nth-last-child(n) {
    ...
}
```

### Only Child

Select elements that are the only child inside a container

```css
div:only-child {
    ...
}
```

### First Of Type

Select elements that are the first of a type inside a container

```css
div:first-of-type {
    ...
}
```

### Last Of Type

Select elements that are the last of a type inside a container

```css
div:last-of-type {
    ...
}
```

### Nth Of Type

Select elements that are the nth of a type inside a container based on the formula

```css
div:nth-of-type(n) {
    ...
}
```

### Nth Last Of Type

Select elements that are the nth of a type inside a container based on the formula counting from the end

```css
div:nth-last-of-type(n) {
    ...
}
```

### Only Of Type

Select elements that are the only of a type inside a container

```css
div:only-of-type {
    ...
}
```

### Not

Select all elements that do not match the selector inside the not selector

```css
div:not(.c) {
    ...
}
```

