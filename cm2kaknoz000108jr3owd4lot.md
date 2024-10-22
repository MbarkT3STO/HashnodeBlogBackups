---
title: "Svelte: Creating Reusable Components and Libraries"
datePublished: Tue Oct 22 2024 10:17:38 GMT+0000 (Coordinated Universal Time)
cuid: cm2kaknoz000108jr3owd4lot
slug: svelte-creating-reusable-components-and-libraries
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729591781308/dcb2c79b-2412-4169-a465-56b0fb0b3302.png
tags: javascript, frontend-development, svelte

---

One of the key strengths of Svelte is its ability to create reusable components that can be shared across different projects or even published as libraries. In this article, we will explore how to build reusable Svelte components, how to structure them for reusability, and how to publish them as a standalone library.

## What Are Reusable Components?

Reusable components are independent UI elements that can be easily integrated into multiple Svelte projects. They encapsulate logic, styling, and functionality, making them modular and maintainable.

## Building a Reusable Component

### Example: Creating a Button Component

Let’s start by creating a simple button component that can be reused across different parts of an application.

```svelte
<!-- Button.svelte -->
<script>
  export let label = 'Click me';
  export let type = 'button';
</script>

<button type={type} class="btn">
  {label}
</button>

<style>
  .btn {
    background-color: #3498db;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  .btn:hover {
    background-color: #2980b9;
  }
</style>
```

### Explanation:

* **Props**: The `label` and `type` props make the button customizable for different use cases.
    
* **Encapsulation**: The styles and functionality are encapsulated within the component, making it easy to reuse without affecting the rest of the application.
    

### Usage Example

```svelte
<script>
  import Button from './Button.svelte';
</script>

<Button label="Submit" type="submit" />
<Button label="Cancel" type="button" />
```

## Structuring a Component for Reusability

When designing reusable components, it’s important to think about flexibility and maintainability.

### Key Principles for Reusability

1. **Props**: Make your component customizable by using props for dynamic data and configuration.
    
2. **Slots**: Use slots to allow users to inject custom content into your component.
    
3. **Events**: Emit custom events to communicate with parent components.
    

### Example: Button with Slots and Events

```svelte
<!-- ButtonWithSlot.svelte -->
<script>
  export let type = 'button';
  export let disabled = false;

  function handleClick() {
    dispatch('click');
  }
</script>

<button type={type} {disabled} on:click={handleClick} class="btn">
  <slot></slot> <!-- Allows custom content inside the button -->
</button>

<style>
  /* Styling similar to previous example */
</style>
```

### Usage Example with Slot

```svelte
<script>
  import ButtonWithSlot from './ButtonWithSlot.svelte';

  function onButtonClick() {
    console.log('Button clicked!');
  }
</script>

<ButtonWithSlot on:click={onButtonClick}>
  <strong>Custom Label</strong>
</ButtonWithSlot>
```

### Explanation:

* **Slot**: Allows the user to insert custom content inside the button, making it highly flexible.
    
* **Event Handling**: The `click` event is dispatched from the button, and the parent component listens to it.
    

## Creating a Component Library

### Setting Up the Project

To create a reusable component library, it’s important to structure your project in a way that makes it easy to maintain, distribute, and install as a package.

1. **Create a new Svelte project** for your library:
    
    ```svelte
    npx degit sveltejs/template my-svelte-library
    cd my-svelte-library
    npm install
    ```
    
2. **Organize components**: Put your reusable components in a `src/lib` folder. This keeps them organized and easy to reference.
    
3. **Bundle the library**: Use a bundler like Rollup to bundle the components for distribution.
    

### Example: `rollup.config.js` for a Svelte Library

```javascript
import svelte from 'rollup-plugin-svelte';
import resolve from '@rollup/plugin-node-resolve';
import pkg from './package.json';

export default {
  input: 'src/lib/index.js', // Entry point for your components
  output: [
    { file: pkg.module, format: 'es' },
    { file: pkg.main, format: 'umd', name: 'MyLibrary' }
  ],
  plugins: [
    svelte(),
    resolve()
  ]
};
```

### Export Components

In your `src/lib/index.js`, export all the components you want to include in the library:

```svelte
export { default as Button } from './Button.svelte';
export { default as ButtonWithSlot } from './ButtonWithSlot.svelte';
```

### Package and Publish

Once your components are ready, you can publish your library to npm:

1. Create a `package.json` file for your library.
    
2. Run the following command to publish:
    
    ```svelte
    npm publish
    ```
    

Now, other developers can install your library using npm:

```svelte
npm install my-svelte-library
```

## Using a Component Library

Once your component library is published, you or other developers can easily install and use it in any Svelte project.

### Example: Using a Component from the Library

```svelte
<script>
  import { Button } from 'my-svelte-library';
</script>

<Button label="Click Me" />
```

### Benefits of Component Libraries

* **Reusability**: Once a library is created, you can use it in multiple projects without repeating code.
    
* **Modularity**: Each component is independent and can be developed, tested, and maintained separately.
    
* **Collaboration**: Component libraries make it easy to share code with other developers or teams.