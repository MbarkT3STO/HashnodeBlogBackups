---
title: "Svelte: State Management"
datePublished: Sun Oct 20 2024 09:51:52 GMT+0000 (Coordinated Universal Time)
cuid: cm2hertl5000009l77byndh7z
slug: svelte-state-management
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729417808620/77efaabf-ccc3-4754-bb6a-e31678416533.png
tags: javascript, frontend-development, svelte

---

## Overview

Managing state efficiently is crucial in any application. Svelte offers several ways to manage state, ranging from local state inside components to shared state between multiple components using stores. In this article, we’ll cover how to handle state in Svelte and introduce stores for shared state management.

## Local State in Svelte

In Svelte, local state is simply defined as a variable inside the `<script>` tag of a component. Any change to the state automatically updates the UI thanks to Svelte’s built-in reactivity.

### Example:

```svelte
<script>
  let count = 0;

  function increment() {
    count += 1;
  }
</script>

<button on:click={increment}>
  Count: {count}
</button>
```

### Reactive Assignments

Svelte automatically tracks dependencies in your variables. For example, when a variable is updated, Svelte re-renders the relevant parts of the DOM.

```svelte
<script>
  let price = 50;
  let quantity = 2;
  let total = price * quantity;

  $: total = price * quantity; // Reactively update total when price or quantity changes
</script>

<p>Total: {total}</p>
<input type="number" bind:value={quantity}>
```

## Shared State with Svelte Stores

For more complex applications where state needs to be shared between multiple components, Svelte provides **stores**. Stores are reactive objects that allow state to be shared and updated across different parts of an application.

### Types of Stores:

1. **Writable Store**: Allows both reading and writing of state.
    
2. **Readable Store**: State can only be read, but not modified directly.
    
3. **Derived Store**: A store whose value is derived from other stores.
    

### Writable Store Example:

1. **Creating a Store**  
    First, import `writable` from Svelte’s `store` package:
    
    ```svelte
    <script>
      import { writable } from 'svelte/store';
    
      // Create a writable store
      let count = writable(0);
    </script>
    
    <button on:click={() => count.update(n => n + 1)}>
      Count: {$count}
    </button>
    ```
    
2. **Using the Store in Another Component**  
    You can use the same store across multiple components by importing it. All components will automatically react to changes in the store.
    
    ```svelte
    <script>
      import { count } from './store';
    </script>
    
    <p>The count is {$count}</p>
    ```
    

### Readable Store Example:

A **readable** store allows you to create state that can only be updated internally, such as fetching data from an API or subscribing to an external data source.

```svelte
<script>
  import { readable } from 'svelte/store';

  const time = readable(new Date(), function start(set) {
    const interval = setInterval(() => {
      set(new Date());
    }, 1000);

    return function stop() {
      clearInterval(interval);
    };
  });
</script>

<p>The current time is: {$time}</p>
```

## Using Context API for Deeply Nested Components

In cases where state needs to be shared between deeply nested components, Svelte provides the **Context API**. This is useful to avoid "prop drilling" (passing props through multiple layers of components).

### Example:

1. **Setting the Context in a Parent Component**:
    
    ```svelte
    <script>
      import { setContext } from 'svelte';
      const name = 'Svelte';
      setContext('name', name);
    </script>
    ```
    
2. **Getting the Context in a Child Component**:
    
    ```svelte
    <script>
      import { getContext } from 'svelte';
      const name = getContext('name');
    </script>
    
    <p>Name from context: {name}</p>
    ```