---
description: >-
  Master the mystical art of Transformation Enchantments - smooth metamorphosis
  spells that gracefully transition elements between different mystical states,
  creating fluid and captivating visual experiences.

theme: "magic"
---

# The Transformation Enchantment Grimoire

## The Ancient Knowledge

Transformation enchantments allow elements to gracefully metamorphose from one mystical state to another, creating smooth and captivating visual transitions. These powerful spells bridge the gap between static styling and full animation magic, providing elegant state changes that enhance user experience.

## Anatomy of a Transformation Spell

### Mystical Property Selection

The specific property you wish to enchant with transformation magic.\
(Only certain properties possess the mystical ability to transform, see [Sacred Animatable Properties](https://www.w3.org/TR/css-transitions-1/#animatable-properties-))

### Temporal Duration Incantation

The length of the transformation spell in mystical time units: 4s or 4000ms

### Timing Rhythm Enchantment

The mystical "cushioning" that controls the transformation's flow, **Optional:** defaults to ease

### Delay Mystical Pause

The number of mystical time units to pause before unleashing the transformation **Optional**

### **Transformation Shorthand Spell**

```css
:root {
    /*transition: property duration ease delay*/
    transition: transform 2s ease-in-out .5s;
}
```

### Multi-Property Transformation Rituals

Separate multiple transformation spells with mystical commas. ~~Avoid the "all" property for transition-property~~ (FORBIDDEN DARK MAGIC)

```css
:root {
    transition:
        transform 2s ease-in-out .5s,
        background 1000ms ease-in;
}
```

## Temporal Duration Wisdom

Three mystical timelines for transformation enchantments

* **< 100 ms :** Instantaneous Flash
* **100 \~ 1000 ms :** Connected Flow
* **10 s :** Disconnected Drift

**250 \~ 300 ms :** The sacred sweet spot for most transformation magic

## Wisdom of the Ancients

* **Single Manifestation :** When you desire something to happen only once in response to a mystical trigger
* **Precise Control :** When you wish to enchant only one or two properties in a given mystical state
* **Resilient Magic :** When transformation spells aren't supported, the change manifests anyway through ancient fallback magic
