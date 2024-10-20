---
title: "Svelte: Working with APIs"
datePublished: Sun Oct 20 2024 10:40:20 GMT+0000 (Coordinated Universal Time)
cuid: cm2hgi59f000309lebo4s8gjl
slug: svelte-working-with-apis
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729420663180/5f34a41b-3329-418c-bf8e-8e9339277acc.png
tags: javascript, frontend-development, svelte

---

## Overview

Interacting with APIs is a common requirement in modern web applications. Svelte makes it straightforward to fetch data from APIs and handle asynchronous operations. In this article, we’ll cover how to make API requests, manage responses, and handle errors effectively in Svelte.

## Making API Requests

You can use the native `fetch` API to make requests in Svelte. The `fetch` function returns a promise, which you can handle using `.then()` or `async/await` syntax.

### Example: Fetching Data from an API

```svelte
<script>
  let users = [];
  let error = '';

  async function fetchUsers() {
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/users');
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      users = await response.json();
    } catch (err) {
      error = err.message;
    }
  }

  // Fetch users when the component mounts
  fetchUsers();
</script>

{#if error}
  <p style="color: red;">Error: {error}</p>
{:else if users.length > 0}
  <ul>
    {#each users as user}
      <li>{user.name}</li>
    {/each}
  </ul>
{:else}
  <p>Loading...</p>
{/if}
```

### Explanation:

* **Async Function**: The `fetchUsers` function is declared as `async`, allowing the use of `await` to wait for the fetch operation to complete.
    
* **Error Handling**: Errors are caught and displayed to the user, providing a clear feedback mechanism.
    

## Handling Form Submissions with APIs

When working with APIs, you may need to send data to a server, such as submitting a form. This typically involves using the `POST` method.

### Example: Submitting Form Data

```svelte
<script>
  let name = '';
  let email = '';
  let message = '';

  async function handleSubmit() {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ name, email }),
    });

    if (response.ok) {
      message = 'Form submitted successfully!';
      name = '';
      email = '';
    } else {
      message = 'Failed to submit form.';
    }
  }
</script>

<form on:submit|preventDefault={handleSubmit}>
  <label for="name">Name:</label>
  <input type="text" id="name" bind:value={name} required>

  <label for="email">Email:</label>
  <input type="email" id="email" bind:value={email} required>

  <button type="submit">Submit</button>
</form>

{#if message}
  <p>{message}</p>
{/if}
```

### Explanation:

* **Submitting Data**: The form data is sent to the API using a `POST` request. The response is checked, and a success or error message is displayed accordingly.
    

## Managing Loading States

When making API requests, it’s essential to manage loading states to enhance user experience. You can use a loading variable to indicate when data is being fetched.

### Example: Managing Loading State

```svelte
<script>
  let users = [];
  let loading = true;
  let error = '';

  async function fetchUsers() {
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/users');
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      users = await response.json();
    } catch (err) {
      error = err.message;
    } finally {
      loading = false; // Set loading to false when the request is complete
    }
  }

  fetchUsers();
</script>

{#if loading}
  <p>Loading...</p>
{:else if error}
  <p style="color: red;">Error: {error}</p>
{:else}
  <ul>
    {#each users as user}
      <li>{user.name}</li>
    {/each}
  </ul>
{/if}
```

## Conclusion

Svelte simplifies working with APIs, allowing you to fetch data, manage responses, and handle errors effectively. With proper loading states and user feedback, you can create a smooth user experience. In the next topic, we’ll explore routing in Svelte applications, enabling navigation between different views.