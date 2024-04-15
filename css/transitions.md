---
description: CSS Transitions
---

# Transitions

## Anatomy of a Transition

### Transition-property&#x20;

the property you want to transition.\
(Only some properties are transitionable, see [goo.gl/Ttk1S2](https://www.w3.org/TR/css-transitions-1/#animatable-properties-))

### Transition-duration

In seconds or milliseconds: 4s or 4000ms&#x20;

### Transition-timing-function

"cushioning" for the transition, **Optional:** defaults to ease

### Transition-delay&#x20;

The number of milli/seconds to delay the transition before firing it **Optional**

### **Transition shorthand**

```css
:root {
    /*transition: property duration ease delay*/
    transition: transform 2s ease-in-out .5s;
}
```

### Transition Multi Properties

Seperate the shorthand transitions with coma. ~~Or use the "all" property for transition-property~~ (DON'T DO IT)



```css
:root {
    transition: 
        transform 2s ease-in-out .5s,
        background 1000ms ease-in;
}
```

## Duration

Three timelines for a transition

* **< 100 ms :** Instantaneous
* **100 \~ 1000 ms :** Connected
* **10 s :** Disconnected

**250 \~ 300 ms :** Sweet spot for many animations

## Summary

* **Single fire :** if  you only want something to happen once.
* **Granularity** : if you would only animate one or two properties in a given state
* **Bulletproof** : if transitions aren't supported, the change happens anyway
