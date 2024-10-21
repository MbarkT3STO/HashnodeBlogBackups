---
title: "Svelte: Advanced State Management with Stores"
datePublished: Mon Oct 21 2024 09:36:46 GMT+0000 (Coordinated Universal Time)
cuid: cm2ito9b0000g09jsbios166t
slug: svelte-advanced-state-management-with-stores
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729502416061/4926327d-4dfd-4ed7-bd5a-c3c174e74416.png
tags: javascript, frontend-development, svelte

---

In Svelte, stores are the official way to manage shared state between components. Stores provide a reactive way to handle global state in your application, enabling components to easily access and modify the state. In this article, we’ll explore advanced concepts related to stores, including writable, readable, and derived stores, along with custom store implementations for more complex scenarios.

## Types of Stores in Svelte

Svelte provides three types of built-in stores:

1. **Writable stores**: For values that can be updated.
    
2. **Readable stores**: For values that are derived or not directly modifiable.
    
3. **Derived stores**: For stores that depend on other stores.
    

Let’s explore each of these in more detail.

### Writable Stores

Writable stores are the most common type and allow you to set and update their value.

### Example: Using a Writable Store

```svelte
<script>
  import { writable } from 'svelte/store';

  // Creating a writable store
  let count = writable(0);

  // Function to increment the count
  function increment() {
    count.update(n => n + 1);
  }
</script>

<h1>{$count}</h1>
<button on:click={increment}>Increment</button>
```

### Explanation:

* **Writable Store**: The `writable` store holds the state, in this case, a `count` starting at `0`.
    
* **Reactive Subscription**: The `$` syntax automatically subscribes to the store and updates the UI when the store’s value changes.
    
* **Updating Store**: The `update` method modifies the current value of the store, allowing for reactive updates.
    

### Writable Store Methods

* `set(value)`: Directly sets the value of the store.
    
* `update(fn)`: Takes a callback function that receives the current value and returns the new value.
    

## Readable Stores

Readable stores are used when the value should be read but not directly modified by components.

### Example: Creating a Readable Store

```svelte
<script>
  import { readable } from 'svelte/store';

  // Readable store that provides a fixed value
  const time = readable(new Date(), function start(set) {
    const interval = setInterval(() => {
      set(new Date());
    }, 1000);

    // Cleanup function
    return function stop() {
      clearInterval(interval);
    };
  });
</script>

<h1>The time is: {$time}</h1>
```

### Explanation:

* **Readable Store**: The `readable` store takes an initial value (in this case, the current time) and a `start` function that updates the value.
    
* **Automatic Updates**: The time updates every second using `setInterval`, and the UI is automatically updated due to the reactive `$` syntax.
    
* **Cleanup**: The `stop` function ensures the interval is cleared when the store is no longer used.
    

## Derived Stores

Derived stores depend on one or more other stores. They automatically update when the underlying stores change.

### Example: Derived Store from Two Writable Stores

```svelte
<script>
  import { writable, derived } from 'svelte/store';

  // Two writable stores
  let firstName = writable('John');
  let lastName = writable('Doe');

  // Derived store that concatenates the two values
  let fullName = derived([firstName, lastName], ([$firstName, $lastName]) => {
    return `${$firstName} ${$lastName}`;
  });
</script>

<p>First Name: <input bind:value={$firstName} /></p>
<p>Last Name: <input bind:value={$lastName} /></p>
<h1>Full Name: {$fullName}</h1>
```

### Explanation:

* **Derived Store**: The `fullName` store derives its value from `firstName` and `lastName`, updating automatically when either of the underlying stores changes.
    
* **Multiple Stores**: You can pass multiple stores as dependencies for a derived store.
    

## Custom Writable Stores

For more complex scenarios, you can create custom writable stores with additional logic for state management.

### Example: Custom Writable Store with Increment and Decrement

```svelte
<script>
  import { writable } from 'svelte/store';

  function createCounter() {
    const { subscribe, set, update } = writable(0);

    return {
      subscribe,
      increment: () => update(n => n + 1),
      decrement: () => update(n => n - 1),
      reset: () => set(0)
    };
  }

  const counter = createCounter();
</script>

<h1>{$counter}</h1>
<button on:click={counter.increment}>Increment</button>
<button on:click={counter.decrement}>Decrement</button>
<button on:click={counter.reset}>Reset</button>
```

### Explanation:

* **Custom Store**: The `createCounter` function returns an object that extends the basic `writable` store with custom methods (`increment`, `decrement`, `reset`).
    
* **Encapsulation**: This allows for more encapsulated and controlled state management.
    

## Using Stores with Components

Stores can be passed as props to components, making it easier to manage shared state.

### Example: Passing a Store as a Prop

```svelte
<!-- Parent.svelte -->
<script>
  import { writable } from 'svelte/store';
  import Child from './Child.svelte';

  const count = writable(0);
</script>

<Child count={count} />

<!-- Child.svelte -->
<script>
  export let count;
</script>

<h1>Count in Child: {$count}</h1>
<button on:click={() => $count += 1}>Increment</button>
```

### Explanation:

* **Prop Passing**: The `count` store is passed as a prop from the parent component to the child component.
    
* **Reactivity**: The child component can directly access and modify the store, and the parent will reflect these changes due to Svelte’s reactivity system.
    

## Conclusion

Stores in Svelte provide a powerful mechanism for managing state across your application. By utilizing writable, readable, and derived stores, you can handle complex state management scenarios efficiently. Custom stores give you even more control, making Svelte’s state management both flexible and intuitive. In the next topic, we’ll explore how to create reusable components and libraries in Svelte.