---
title: "Svelte: Routing"
datePublished: Sun Oct 20 2024 11:07:02 GMT+0000 (Coordinated Universal Time)
cuid: cm2hhghf4000009jtcdtc7uw1
slug: svelte-routing
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729422140320/a54eeef2-0bd6-4132-8ba8-49ff2b937398.png
tags: javascript, frontend-development, svelte

---

Routing is essential for navigating between different views in a web application. Svelte provides a powerful yet simple way to handle routing using the `svelte-routing` library. In this article, we’ll cover how to set up routing in your Svelte application, manage routes, and create dynamic routes.

## Installing the Routing Library

To get started with routing in Svelte, you need to install the `svelte-routing` library.

### Installation

```svelte
npm install svelte-routing
```

## Setting Up the Router

After installing the library, you can set up the router in your main application component.

### Example: Basic Router Setup

```svelte
<script>
  import { Router, Route, Link } from 'svelte-routing';
  import Home from './Home.svelte';
  import About from './About.svelte';
</script>

<Router>
  <nav>
    <Link to="/">Home</Link>
    <Link to="/about">About</Link>
  </nav>

  <Route path="/" component={Home} />
  <Route path="/about" component={About} />
</Router>
```

### Explanation:

* **Router Component**: The `Router` component wraps your entire application and enables routing.
    
* **Link Component**: The `Link` component creates navigation links that update the URL without reloading the page.
    
* **Route Component**: The `Route` component specifies which component to render for a given path.
    

## Creating Dynamic Routes

You can create dynamic routes to handle parameters in the URL. For example, you might want to display user profiles based on their IDs.

### Example: Dynamic Route Setup

```svelte
<script>
  import { Router, Route, Link } from 'svelte-routing';
  import User from './User.svelte';

  let users = [{ id: 1, name: 'John' }, { id: 2, name: 'Jane' }];
</script>

<Router>
  <nav>
    {#each users as user}
      <Link to={`/user/${user.id}`}>{user.name}</Link>
    {/each}
  </nav>

  <Route path="/user/:id" component={User} />
</Router>
```

### Explanation:

* **Dynamic Path**: The `:id` in the route path indicates that it’s a dynamic parameter.
    
* **User Component**: The `User` component will receive the `id` parameter from the URL.
    

## Accessing Route Parameters

In the component corresponding to a dynamic route, you can access the route parameters using the `useParams` hook.

### Example: Using Route Parameters

```svelte
<script>
  import { useParams } from 'svelte-routing';

  const params = useParams();
  let userId = params.id; // Access the dynamic route parameter
</script>

<h1>User ID: {userId}</h1>
```

## Nested Routing

Svelte allows for nested routing, where routes can have child routes. This is useful for organizing complex applications.

### Example: Nested Routes

```svelte
<script>
  import { Router, Route, Link } from 'svelte-routing';
  import UserProfile from './UserProfile.svelte';
  import UserPosts from './UserPosts.svelte';
</script>

<Router>
  <nav>
    <Link to="/user/1/profile">User 1 Profile</Link>
    <Link to="/user/1/posts">User 1 Posts</Link>
  </nav>

  <Route path="/user/:id/profile" component={UserProfile} />
  <Route path="/user/:id/posts" component={UserPosts} />
</Router>
```

## Conclusion

Svelte’s routing capabilities allow you to build dynamic and navigable web applications easily. By using the `svelte-routing` library, you can set up basic routing, manage dynamic parameters, and create nested routes effectively. In the next topic, we’ll explore state management in Svelte using Svelte stores.