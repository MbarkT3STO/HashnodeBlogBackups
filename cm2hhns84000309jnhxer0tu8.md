---
title: "Svelte: State Management with Stores"
datePublished: Sun Oct 20 2024 11:12:42 GMT+0000 (Coordinated Universal Time)
cuid: cm2hhns84000309jnhxer0tu8
slug: svelte-state-management-with-stores
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729422472322/baa9fe8b-cafa-4e0c-80e6-0a2c31bc0e1f.png
tags: javascript, frontend-development, svelte

---

State management is a crucial aspect of building responsive applications. Svelte provides a simple and efficient way to manage state using stores. In this article, we’ll explore how to create and use stores in Svelte, including writable, readable, and derived stores.

## What are Svelte Stores?

Stores are reactive data structures that allow you to share state between components without prop drilling. Svelte offers three main types of stores: **writable**, **readable**, and **derived**.

### Writable Stores

Writable stores allow you to create a store that can be updated. They are suitable for managing mutable state.

#### Creating a Writable Store

```svelte
<script>
  import { writable } from 'svelte/store';

  // Create a writable store
  const count = writable(0);
</script>

<h1>Count: {$count}</h1>
<button on:click={() => count.update(n => n + 1)}>Increment</button>
<button on:click={() => count.set(0)}>Reset</button>
```

### Explanation:

* **writable**: The `writable` function creates a store that holds a value and allows updates.
    
* **$count**: The dollar sign syntax (`$`) automatically subscribes to the store, making it reactive.
    

## Readable Stores

Readable stores are used for values that should not be directly modified. They are suitable for derived state or values that come from external sources.

### Creating a Readable Store

```svelte
<script>
  import { readable } from 'svelte/store';

  // Create a readable store
  const time = readable(new Date(), (set) => {
    const interval = setInterval(() => {
      set(new Date());
    }, 1000);

    // Cleanup function to stop the interval when no longer needed
    return () => clearInterval(interval);
  });
</script>

<h1>Current Time: {$time.toLocaleTimeString()}</h1>
```

### Explanation:

* **readable**: The `readable` function creates a store that cannot be modified directly. It requires a subscribe function to update its value.
    
* **Cleanup Function**: The returned function is called when the store is no longer needed, allowing you to clean up resources.
    

## Derived Stores

Derived stores are used to compute values based on one or more other stores. They are useful for creating reactive computations.

### Creating a Derived Store

```svelte
<script>
  import { writable, derived } from 'svelte/store';

  const count = writable(0);
  const doubleCount = derived(count, $count => $count * 2);
</script>

<h1>Count: {$count}</h1>
<h1>Double Count: {$doubleCount}</h1>
<button on:click={() => count.update(n => n + 1)}>Increment</button>
```

### Explanation:

* **derived**: The `derived` function creates a new store that computes its value based on the input store(s).
    
* **Reactive Calculation**: The derived store automatically updates whenever the input store changes.
    

## Using Multiple Stores

You can combine multiple stores to create more complex state management solutions.

### Example: Combining Stores

```svelte
<script>
  import { writable, derived } from 'svelte/store';

  const firstName = writable('John');
  const lastName = writable('Doe');
  const fullName = derived(
    [firstName, lastName],
    ([$firstName, $lastName]) => `${$firstName} ${$lastName}`
  );
</script>

<h1>Full Name: {$fullName}</h1>
<input bind:value={$firstName} placeholder="First Name">
<input bind:value={$lastName} placeholder="Last Name">
```

### Explanation:

* **Combining Stores**: The derived store combines the values of `firstName` and `lastName` to compute `fullName`.
    
* **Reactive Updates**: Changes to either input will automatically update the full name displayed.
    

## Conclusion

Svelte's store system provides a powerful yet simple way to manage state in your applications. By utilizing writable, readable, and derived stores, you can effectively handle complex state management scenarios. In the next topic, we’ll explore animations and transitions in Svelte, enhancing the user experience with dynamic visual effects.