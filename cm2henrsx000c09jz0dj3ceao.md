---
title: "Svelte: Understanding the Components"
datePublished: Sun Oct 20 2024 09:48:43 GMT+0000 (Coordinated Universal Time)
cuid: cm2henrsx000c09jz0dj3ceao
slug: svelte-understanding-the-components
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729416364599/5cbf354e-b3e1-4642-8913-ca46cba72e54.png
tags: javascript, frontend-development, svelte

---

## Overview

Components are the building blocks of any Svelte application. In this article, we’ll explore how to create and use components in Svelte, how to pass data between them using props, and how Svelte’s reactivity works.

## What Are Svelte Components?

Svelte components are self-contained units that define their structure (HTML), appearance (CSS), and behavior (JavaScript) in a single file with the `.svelte` extension. This allows for a clear and organized way to build applications.

### Basic Component Structure

A Svelte component typically consists of three main sections:

* **Script**: Contains the JavaScript logic for the component.
    
* **Markup**: Defines the HTML structure.
    
* **Styles**: Optional scoped CSS for styling the component.
    

Example of a simple Svelte component:

```svelte
<script>
  let name = 'world';
</script>

<style>
  h1 {
    color: blue;
  }
</style>

<h1>Hello {name}!</h1>
```

### Creating a New Component

1. **Creating a File**  
    Components are stored in the `src` folder. To create a new component, simply create a new `.svelte` file, for example, `src/Hello.svelte`.
    
2. **Using the Component**  
    You can use this component in other files by importing it:
    
    ```svelte
    <script>
      import Hello from './Hello.svelte';
    </script>
    
    <Hello />
    ```
    

## Passing Data Between Components: Props

Props allow you to pass data from a parent component to a child component. In Svelte, you can define props using the `export` keyword.

### Example:

In the `Hello.svelte` component, define a prop `name`:

```svelte
<script>
  export let name;
</script>

<h1>Hello {name}!</h1>
```

In the parent component, pass the `name` prop when using `<Hello>`:

```svelte
<Hello name="Svelte" />
```

The result will display: "Hello Svelte!"

## Reactivity in Svelte

Svelte components are automatically reactive. This means any change to the component’s state will trigger an update to the DOM.

### Example of Reactivity:

```svelte
<script>
  let count = 0;

  function increment() {
    count += 1;
  }
</script>

<button on:click={increment}>
  Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
```

Every time the button is clicked, the `count` variable is updated, and Svelte automatically updates the DOM without any manual intervention.

## Svelte Lifecycle Hooks

Svelte provides lifecycle hooks that allow you to run code at specific times during a component’s existence.

### Common Lifecycle Hooks:

* `onMount`: Runs when the component is first inserted into the DOM.
    
* `onDestroy`: Runs when the component is removed from the DOM.
    

Example:

```svelte
<script>
  import { onMount, onDestroy } from 'svelte';

  onMount(() => {
    console.log('Component mounted');
  });

  onDestroy(() => {
    console.log('Component destroyed');
  });
</script>
```

## Conclusion

Svelte components offer a straightforward way to build interactive UIs. With their clear structure, ease of passing props, and built-in reactivity, they make web development simple and efficient. In the next topic, we’ll dive deeper into managing state within Svelte using stores and contexts.