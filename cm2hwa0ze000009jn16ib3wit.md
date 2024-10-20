---
title: "Svelte: Animations and Transitions"
datePublished: Sun Oct 20 2024 18:01:55 GMT+0000 (Coordinated Universal Time)
cuid: cm2hwa0ze000009jn16ib3wit
slug: svelte-animations-and-transitions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729447181452/e8410464-a2df-4878-864b-c495f4b347ef.avif
tags: javascript, frontend-development, svelte

---

Animations and transitions add life to your web applications, creating smooth interactions and a more engaging user experience. Svelte provides built-in directives for easily adding animations and transitions to your components. In this article, we’ll cover how to implement transitions, keyframe animations, and custom animations in Svelte.

## Transitions in Svelte

Transitions in Svelte can be applied to elements when they enter or leave the DOM. Svelte offers built-in transition functions like `fade`, `slide`, `scale`, and `fly`.

### Example: Using `fade` Transition

```svelte
<script>
  import { fade } from 'svelte/transition';
  let visible = true;
</script>

<button on:click={() => visible = !visible}>
  Toggle
</button>

{#if visible}
  <div transition:fade>
    <p>This paragraph will fade in and out.</p>
  </div>
{/if}
```

### Explanation:

* `fade` Transition: The `fade` function applies a simple fade-in/fade-out effect when the element is added or removed from the DOM.
    
* **Reactive Toggle**: The visibility of the paragraph is controlled by the `visible` variable, triggering the transition.
    

## Other Built-In Transitions

Svelte provides several other transition functions to create different effects.

### Example: `slide` and `fly` Transitions

```svelte
<script>
  import { slide, fly } from 'svelte/transition';
  let showSlide = true;
  let showFly = true;
</script>

<button on:click={() => showSlide = !showSlide}>Toggle Slide</button>
<button on:click={() => showFly = !showFly}>Toggle Fly</button>

{#if showSlide}
  <div transition:slide>
    <p>This paragraph slides in and out.</p>
  </div>
{/if}

{#if showFly}
  <div transition:fly={{ y: 200, duration: 800 }}>
    <p>This paragraph flies in and out.</p>
  </div>
{/if}
```

### Explanation:

* `slide` Transition: This transition slides the element in and out of view.
    
* `fly` Transition: The `fly` transition allows for movement along the `x` and `y` axes with customizable duration and distance.
    

## Custom Transitions

If the built-in transitions don’t fit your needs, you can create custom transitions by defining your own logic.

### Example: Custom Transition

```svelte
<script>
  import { cubicInOut } from 'svelte/easing';

  function customTransition(node, { delay = 0, duration = 400 }) {
    return {
      delay,
      duration,
      css: t => `
        transform: scale(${t});
        opacity: ${t};
      `
    };
  }
</script>

<div transition:customTransition>
  <p>This paragraph uses a custom transition.</p>
</div>
```

### Explanation:

* **Custom Transition Function**: You define a custom transition by returning an object with `delay`, `duration`, and a `css` function that describes how the styles should change over time.
    
* **Easing Function**: Svelte provides several easing functions like `cubicInOut` to control the transition speed.
    

## Keyframe Animations

Svelte also supports keyframe animations using the `animate` directive. This is useful for animating changes in element properties over time.

### Example: Keyframe Animation with `flip`

```svelte
<script>
  import { flip } from 'svelte/animate';

  let items = [1, 2, 3, 4, 5];
  function shuffle() {
    items = items.sort(() => Math.random() - 0.5);
  }
</script>

<button on:click={shuffle}>Shuffle</button>

<ul>
  {#each items as item (item)}
    <li animate:flip>{item}</li>
  {/each}
</ul>
```

### Explanation:

* `flip` Animation: The `flip` animation smoothly animates changes in the position of elements when their order changes.
    
* **Unique Key**: The `(item)` after the `each` loop ensures that Svelte can track each element uniquely for animation purposes.
    

## Animating Between Two States

You can animate elements as they transition between two states using the `animate` directive.

### Example: State Animation

```svelte
<script>
  let expanded = false;
</script>

<button on:click={() => expanded = !expanded}>
  {expanded ? 'Collapse' : 'Expand'}
</button>

<div style="overflow: hidden; background-color: lightgray;" animate:flip>
  <p style="height: {expanded ? '200px' : '50px'};">
    {expanded ? 'Expanded view with more content.' : 'Collapsed view.'}
  </p>
</div>
```

### Explanation:

* **State Change Animation**: The height of the paragraph changes based on the `expanded` variable, and Svelte smoothly animates the transition between states.
    

## Conclusion

Svelte makes it incredibly easy to add animations and transitions to your application, enhancing the user experience. Whether you're using built-in transitions like `fade` or creating custom transitions, Svelte offers flexibility to create dynamic interfaces. In the next topic, we’ll explore server-side rendering with SvelteKit to optimize performance and SEO.