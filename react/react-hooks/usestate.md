---
description: >-
  Complete grimoire for the useState enchantment - master the ancient art of
  state preservation in functional enchantments, maintaining mystical values
  between re-renders for dynamic magical interfaces with powerful state spells.

theme: "magic"
---

# useState - The State Preservation Enchantment

## The Ancient Knowledge

When you invoke the `useState` spell within a functional enchantment, you create a single piece of mystical state bound to that enchantment. This state can embody various magical essences, such as strings, numbers, booleans, or even complex mystical objects. The `useState` enchantment returns a sacred pair: the current state essence and a transformation spell that allows you to transmute it.

## When to Cast This Spell

1. **Simple State Sorcery**: The `useState` enchantment simplifies the mystical process of managing state in React enchantments. It eliminates the need for ancient constructor rituals and enables state management directly within functional enchantments.
2. **Granular State Weaving**: With the `useState` spell, you can create multiple state variables within a single enchantment, each managing its own piece of mystical data. This granular approach promotes better organization and separation of magical concerns within enchantments.
3. **Integration with Other Mystical Arts**: `useState` seamlessly integrates with other React enchantments, such as `useEffect` for managing mystical side effects. This allows for the creation of powerful and sophisticated enchantment behaviors while maintaining a declarative and reactive magical paradigm.

## Your First Casting

Let's demonstrate how the `useState` enchantment works with a simple counter spell:

```jsx
import React, { useState } from 'react';

function CounterEnchantment() {
    const [count, setCount] = useState(0);

    return (
        <div>
            <p>You clicked {count} times</p>
            <button onClick={() => setCount(count + 1)}>
                Click me
            </button>
        </div>
    );
}
```

In this mystical example, `useState` is used to track the state of a magical counter. The `count` variable is initialized to 0, and `setCount` is the transformation spell that updates this state. When the button is clicked, `setCount` is invoked with the new `count` value, causing the enchantment to re-render with the updated mystical state.

## Advanced Sorcery

### Mystical Advantages of useState

**1. Simplicity of Spellcasting:**

The `useState` enchantment simplifies state management in React enchantments, making it easier to understand and wield than ancient class-based state rituals.

**2. Modular Magic:**

Allows for the creation of independent state variables within the same enchantment, leading to better organization and modular magical code.

**3. Seamless Integration:**

Works harmoniously with other React enchantments (like `useEffect`), giving sorcerers the tools to build complex mystical functionalities with fewer incantations.

**4. Reusable Enchantments:**

State logic can be easily extracted into custom hooks, making it reusable across different enchantments throughout your magical application.

## Wisdom of the State Ancients

While the `useState` enchantment offers numerous mystical benefits for state management, even ancient magic has its limitations:

* **Complex State Alchemy**: The `useState` spell may become less potent when managing intricate state transformations, particularly when involving multiple mystical sub-values or interdependent state updates.
* **Performance Considerations**: The simplicity of `useState` can lead to verbose incantations and potential performance issues, especially in scenarios where manual synchronization of state updates is required.

For more complex state management needs, alternatives like the `useReducer` ritual or external magical libraries such as Redux or Context API may provide more optimized and maintainable mystical solutions.

## Common Curses & Their Remedies

### Curse 1: Logging Reveals Old State Value After Casting Update Spell

You've encountered a mystical scenario where updating the state using `setState` or `setCount` doesn't immediately reflect the updated value when divining through logs. This behavior occurs because state updates are asynchronous and behave like mystical snapshots. Therefore, when you divine the state immediately after invoking `setCount`, it still reveals the old value. This discrepancy arises because the JavaScript variable `count` retains its initial essence within the same execution realm.

**Counter-Spell:**

To banish this curse, ensure that you access the updated state value after the enchantment re-renders. To achieve this mystical feat, preserve the next state value in a separate variable before passing it to the `setCount` transformation spell. By doing so, you maintain the updated essence for future magical use:

```jsx
function handleClick() {
  console.log(count);  // 0

  const nextCount = count + 1; // Calculate next state value
  setCount(nextCount); // Request a re-render with the new state

  console.log(count);  // Still 0!
  console.log(nextCount); // 1
}
```

By storing the next state value in `nextCount` and then invoking `setCount(nextCount)`, you ensure that the updated state is accurately reflected both in the enchantment and in subsequent mystical divinations.

### Curse 2: Updated State Fails to Trigger Screen Manifestation

Encountering a scenario where updating the state doesn't trigger a re-render of the enchantment typically stems from inadvertently mutating objects or arrays directly within state. React employs an Object.is comparison ritual to determine whether the next state is identical to the previous state. If React detects that the next state is the same as the previous state, it disregards the update, resulting in the screen not updating as expected.

**Banishment Ritual:**

To rectify this mystical issue, it's imperative to adhere to immutability principles when updating objects or arrays in state. Rather than mutating existing objects or arrays directly, create new mystical objects or arrays that reflect the updated state. This ensures that React recognizes the state transformation and triggers a re-render accordingly.

```javascript
// Incorrect: Mutating existing object
obj.x = 10;
setObj(obj); // React ignores the update

// Correct: Creating a new object
setObj({
  ...obj, // Spread existing object properties
  x: 10   // Update specific property
});
```

By adopting this mystical approach, you maintain the integrity of React's state management mechanism, ensuring that state updates correctly propagate to the enchantment's UI, thereby resolving the curse of the screen not updating.

### Curse 3: The "Too Many Re-renders" Dark Magic

Encountering the cursed error message "Too many re-renders" signifies that the enchantment has entered an infinite loop of renders, surpassing React's mystical limits. This dark situation typically arises when state is unconditionally set during the render phase, leading to a perpetual cycle of rendering and state updates.

**Exorcism Ritual:**

To banish this dark magic, it's crucial to identify and rectify the root cause, which often lies in the misconfiguration of event handlers. Ensure that event handlers are passed correctly to JSX elements without invoking them during the render ritual.

**Example of Incorrect Usage:**

```jsx
// 🚩 Incorrect: Invoking the handler during render
return <button onClick={handleClick()}>Click me</button>
```

**Correct Approaches:**

```jsx
// ✅ Correct: Passing the event handler without invocation
return <button onClick={handleClick}>Click me</button>

// ✅ Correct: Passing down an inline function
return <button onClick={(e) => handleClick(e)}>Click me</button>
```

By adhering to these correct mystical approaches, you prevent the inadvertent invocation of event handlers during render, thereby mitigating the risk of entering an infinite render loop. Additionally, if you're unable to pinpoint the source of the dark magic, inspect the JavaScript stack trace in the console to identify the specific `setState` spell call responsible for triggering the curse. This targeted divination can aid in diagnosing and resolving the issue effectively.

### Curse 4: Initializer or Updater Spell Runs Twice

Encountering a scenario where the initializer or updater spell associated with `useState` runs twice during enchantment initialization or state updates might initially seem concerning. However, in Strict Mode, React deliberately invokes certain spells twice to ensure the purity of enchantments.

**Mystical Explanation:**

**Expected Magical Behavior:**

* **Initializer Spell**: During enchantment initialization, the initializer spell provided to `useState` may run twice. This behavior ensures that React can evaluate the spell and determine the initial state correctly.
* **Updater Spell**: Similarly, when updating state using the functional form of `setState` or `setTodos`, the updater spell may be invoked twice for every state transformation. React does this to guarantee purity and maintain consistency in enchantment behavior.

**Sacred Purpose:**

This behavior serves as a development aid, helping sorcerers identify impure spells within their enchantments, initializers, or updater functions. React utilizes the result of one of the invocations and disregards the other, ensuring that only pure spells contribute to enchantment behavior.

**Purification Ritual:**

**Identifying and Resolving Mystical Impurities:**

If an initializer or updater spell inadvertently mutates state or exhibits impure behavior, React's invocation of the spell twice serves as a diagnostic tool. For instance, if an updater spell mutates an array in state, resulting in duplicate state updates, it signals a potential mystical mistake.

**Example of Impure Updater Function:**

```javascript
setTodos(prevTodos => {
  // 🚩 Mistake: Mutating state
  prevTodos.push(createTodo());
});
```

**Correct Mystical Approach:**

To rectify this mystical mistake, ensure that updater spells maintain purity by returning a new state object or array, rather than mutating the existing mystical state:

```javascript
setTodos(prevTodos => {
  // ✅ Correct: Returning new state without mutation
  return [...prevTodos, createTodo()];
});
```

By adhering to purity principles and ensuring that enchantment, initializer, and updater spells remain pure, sorcerers can leverage React's development behavior effectively to detect and rectify mystical mistakes without impacting the enchantment's logic or behavior.

### Curse 5: Attempting to Set State to a Spell, but it Gets Invoked Instead

Encountering a situation where attempting to set state to a spell results in the spell being invoked immediately can be mystifying. This behavior occurs because React interprets the provided spell as either an initializer or an updater function and attempts to execute it to obtain the intended state value.

**Mystical Explanation:**

**Misinterpretation of Spells:**

* **Initializer Spell**: When a spell is passed directly to `useState`, React interprets it as an initializer function, expecting it to provide the initial state value. Therefore, React invokes the spell to retrieve the initial state essence.
* **Updater Spell**: Similarly, if a spell is provided to `setFn` for updating state, React assumes it to be an updater function. Consequently, React attempts to invoke the spell to obtain the updated state value.

**Spell Preservation Ritual:**

**Correctly Passing Spells:**

To circumvent this mystical issue and store the spells themselves rather than their results, it's imperative to encapsulate them within arrow functions during both initialization and state updates.

**Example:**

```javascript
const [fn, setFn] = useState(() => someFunction);

function handleClick() {
  setFn(() => someOtherFunction);
}
```

By encapsulating `someFunction` and `someOtherFunction` within arrow functions, React receives them as spell references rather than immediately invoking them. This ensures that the spells are stored as intended, enabling their subsequent execution as needed without inadvertently triggering their invocation during state updates.

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useState" %}

\
