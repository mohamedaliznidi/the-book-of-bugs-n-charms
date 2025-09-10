---
description: >-
  Master the mystical art of CSS Element Targeting Spells. Learn to precisely
  target HTML elements using basic selectors, combination sorcery, attribute
  enchantments, and pseudo-class mystical powers for styling magic.
---

# Element Targeting Spells

## The Ancient Knowledge

Element targeting spells are the fundamental mystical arts of CSS that allow you to precisely select and enchant HTML elements. These powerful incantations form the foundation of all styling magic, enabling you to cast visual spells upon specific elements, groups of elements, or elements in particular mystical states.

## Basic Element Targeting Enchantments

### Universal Mystical Selector

Casts spells upon all elements in the mystical realm

```css
* {
		...
}
```

### Type Element Targeting Spell

Target elements of that mystical type

```css
div {
    ...
}
```

### Class Mystical Identifier

Target elements bearing that mystical class enchantment

```css
.class {
    ...
}
```

### Id Unique Mystical Identifier

Target elements with that unique mystical id

```css
#id {
    ...
}
```

## Combination Targeting Sorcery

### Descendant Mystical Lineage

Target elements that are mystical descendants of the first element

```css
div a {
    ...
}
```

### Direct Child Mystical Bond

Target elements that are direct mystical children of the first element

```css
div > a {
    ...
}
```

### General Sibling Mystical Kinship

Target elements that are mystical siblings of the first element and come after the first element

```css
div ~ a {
    ...
}
```

### Adjacent Sibling Mystical Proximity

Target elements that are mystical siblings of the first element and come directly after the first element

```css
div + a {
    ...
}
```

### Or Mystical Choice Spell

Target elements that match any selector in the mystical list

```css
div, a {
    ...
}
```

### And Mystical Combination Spell

Target elements that match all the mystical selector combinations

```css
div.c {
    ...
}
```

## Attribute Mystical Property Selectors

### Has Mystical Property Spell

Target elements that possess that mystical attribute

```css
[a] {
    ...
}
```

### Exact Mystical Value Match

Target elements that have that mystical attribute with exactly that enchanted value

```css
[a = "1"] {
    ...
}
```

### Begins With Mystical Pattern

Target elements that have that mystical attribute which start with that enchanted value

```css
[a^= "1"] {
    ...
}
```

### Ends With Mystical Pattern

Target elements that have that mystical attribute which end with that enchanted value

```css
[a$= "1"] {
    ...
}
```

### Substring Mystical Pattern

Target elements that have that mystical attribute which contain that enchanted value anywhere

```css
[a*= "1"] {
    ...
}
```

## Pseudo Element Mystical Creation Spells

### Before Mystical Manifestation

Conjures an empty mystical element directly before the children of selected element

```css
div::before {
    ...
}
```

### After Mystical Manifestation

Conjures an empty mystical element directly after the children of selected element

```css
div::after {
    ...
}
```

## Pseudo Class (State) Mystical Selectors

### Hover Mystical Interaction

Target elements that are touched by the mystical mouse cursor

```css
div:hover {
    ...
}
```

### Focus Mystical Attention

Target elements that are receiving mystical focus energy.

```css
div:focus {
    ...
}
```

### Active Mystical Engagement

Target elements that are in an activated mystical state.

```css
div:focus {
    ...
}
```

### Required Mystical Necessity

Target elements that are mystically required

```css
div:required {
    ...
}
```

### Checked Mystical Selection

Target elements that are in a checked mystical state

```css
div:checked {
    ...
}
```

### Disabled Mystical Dormancy

Target elements that are in a disabled mystical state

```css
div:disabled {
    ...
}
```

## Pseudo Class (Position/Other) Mystical Selectors

### First Child Mystical Primacy

Target elements that are the first mystical child inside a container

```css
div:first-child {
    ...
}
```

### Last Child Mystical Finality

Target elements that are the last mystical child inside a container

```css
div:last-child {
    ...
}
```

### Nth Child Mystical Formula

Target elements that are the nth mystical child inside a container based on the enchanted formula

```css
div:nth-child(n) {
    ...
}
```

### Nth Last Child Mystical Reverse Formula

Target elements that are the nth mystical child inside a container based on the enchanted formula counting from the end

```css
div:nth-last-child(n) {
    ...
}
```

### Only Child Mystical Solitude

Target elements that are the only mystical child inside a container

```css
div:only-child {
    ...
}
```

### First Of Type Mystical Category Primacy

Target elements that are the first of a mystical type inside a container

```css
div:first-of-type {
    ...
}
```

### Last Of Type Mystical Category Finality

Target elements that are the last of a mystical type inside a container

```css
div:last-of-type {
    ...
}
```

### Nth Of Type Mystical Category Formula

Target elements that are the nth of a mystical type inside a container based on the enchanted formula

```css
div:nth-of-type(n) {
    ...
}
```

### Nth Last Of Type Mystical Category Reverse Formula

Target elements that are the nth of a mystical type inside a container based on the enchanted formula counting from the end

```css
div:nth-last-of-type(n) {
    ...
}
```

### Only Of Type Mystical Category Solitude

Target elements that are the only of a mystical type inside a container

```css
div:only-of-type {
    ...
}
```

### Not Mystical Exclusion Spell

Target all elements that do not match the selector inside the mystical exclusion spell

```css
div:not(.c) {
    ...
}
```

