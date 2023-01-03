# How to Create a Generic Class in TypeScript?

## **Introduction**

TypeScript is a popular programming language that is a super set of JavaScript. It adds features such as static typing, classes, and interfaces, making it easier to write scalable and maintainable code. In this article, we will learn how to create a generic class in TypeScript.

A generic class is a class that can work with multiple types. It allows you to create a class that is flexible and can be used with different data types without the need to create separate classes for each data type.

## **Syntax**

To create a generic class in TypeScript, you need to use the `<T>` syntax. The `T` stands for the type parameter and can be any letter or symbol. Here is the basic syntax for creating a generic class:

```typescript
class MyClass<T> {
  // class body
}
```

You can also specify multiple type parameters by separating them with a comma:

```typescript
class MyClass<T, U> {
  // class body
}
```

## **Example**

Now, let's see how to create a generic class with an example. Suppose we want to create a simple stack class that can store and retrieve elements in a last-in-first-out (LIFO) order. We can create a `Stack` class with a generic type parameter `T` as follows:

```typescript
class Stack<T> {
  private items: T[] = [];

  push(item: T) {
    this.items.push(item);
  }

  pop(): T {
    return this.items.pop();
  }
}
```

The `Stack` class has a private array `items` of type `T[]` to store the elements. It also has two methods: `push` to add an element to the stack and `pop` to remove and return the last element.

# **Usage**

To use the `Stack` class, we need to specify the type of the elements it will contain. For example, if we want to create a stack of numbers, we can do the following:

```typescript
const stack = new Stack<number>();
stack.push(1);
stack.push(2);
stack.push(3);

console.log(stack.pop()); // Output: 3
console.log(stack.pop()); // Output: 2
console.log(stack.pop()); // Output: 1
```

We can also use the `Stack` class with other data types such as strings or objects. For example:

```typescript
const stack = new Stack<string>();
stack.push("Hello");
stack.push("World");

console.log(stack.pop()); // Output: "World"
console.log(stack.pop()); // Output: "Hello"

const stack = new Stack<{ name: string, age: number }>();
stack.push({ name: "John", age: 30 });
stack.push({ name: "Jane", age: 25 });

console.log(stack.pop().name); // Output: "Jane"
console.log(stack.pop().name); // Output: "John"
```

## **Constraints**

Sometimes you may want to add constraints to the type parameter to ensure that it has certain properties or methods. To do this, you can use the `extends` keyword followed by a type.

For example, suppose we want to ensure that the elements in our `Stack` class have a `length` property. We can do this by adding a constraint to the type parameter:

```typescript
class Stack<T extends { length: number }> {
  private items: T[] = [];

  push(item: T) {
    this.items.push(item);
  }

  pop(): T {
    return this.items.pop();
  }
}
```

Now, the `Stack` class can only be used with types that have a `length` property. If we try to use it with a type that doesn't have this property, we will get a compile-time error.

## Real-World Examples

### Linked List

A linked list is a linear data structure where each element is a separate object and is connected to the next element through a reference.

Here is an example of a generic `LinkedList` class in TypeScript:

```typescript
class LinkedList<T> {
  private head: Node<T> | null = null;
  private tail: Node<T> | null = null;
  private length = 0;

  append(value: T) {
    const node = new Node<T>(value);
    if (this.tail) {
      this.tail.next = node;
    }
    this.tail = node;
    if (!this.head) {
      this.head = node;
    }
    this.length++;
  }

  prepend(value: T) {
    const node = new Node<T>(value);
    if (this.head) {
      node.next = this.head;
    }
    this.head = node;
    if (!this.tail) {
      this.tail = node;
    }
    this.length++;
  }

  delete(value: T) {
    if (!this.head) {
      return;
    }
    let current = this.head;
    let previous: Node<T> | null = null;
    while (current) {
      if (current.value === value) {
        if (previous) {
          previous.next = current.next;
        } else {
          this.head = current.next;
        }
        if (!current.next) {
          this.tail = previous;
        }
        this.length--;
      }
      previous = current;
      current = current.next;
    }
  }

  find(value: T) {
    if (!this.head) {
      return null;
    }
    let current = this.head;
    while (current) {
      if (current.value === value) {
        return current;
      }
      current = current.next;
    }
    return null;
  }
}

class Node<T> {
  value: T;
  next: Node<T> | null = null;

  constructor(value: T) {
    this.value = value;
  }
}
```

The `LinkedList` class has a generic type parameter `T` that specifies the type of the values stored in the list. It has methods to append, prepend, delete, and find elements in the list.

To use the `LinkedList` class, we can specify the type of the elements it will contain. For example, if we want to create a linked list of numbers:

```typescript
const list = new LinkedList<number>();
list.append(1);
list.append(2);
list.append(3);

console.log(list.find(2)); // Output: Node { value: 2, next: Node { value: 3, next: null } }
```

We can also use the `LinkedList` class with other data types such as strings or objects:

```typescript
const list = new LinkedList<string>();
list.append("Hello");
list.append("World");

console.log(list.find("Hello")); // Output: Node { value: "Hello", next: Node { value: "World", next: null } }

const list = new LinkedList<{ name: string, age: number }>();
list.append({ name: "John", age: 30 });
list.append({ name: "Jane", age: 25 });

console.log(list.find({ name: "Jane", age: 25 })); // Output: Node { value: { name: "Jane", age: 25 }, next: null }
```

In this example, we created a linked list of strings and a linked list of objects and used the `find` method to search for elements in the list.

### Priority Queue

A priority queue is a data structure where each element has a priority and the elements are stored in a way that the element with the highest priority is always at the front of the queue.

Here is an example of a generic `PriorityQueue` class in TypeScript:

```typescript
class PriorityQueue<T> {
  private items: { element: T, priority: number }[] = [];

  enqueue(element: T, priority: number) {
    const queueElement = { element, priority };
    if (this.isEmpty()) {
      this.items.push(queueElement);
    } else {
      let added = false;
      for (let i = 0; i < this.items.length; i++) {
        if (queueElement.priority > this.items[i].priority) {
          this.items.splice(i, 0, queueElement);
          added = true;
          break;
        }
      }
      if (!added) {
        this.items.push(queueElement);
      }
    }
  }

  dequeue() {
    return this.items.shift();
  }

  peek() {
    return this.items[0];
  }

  isEmpty() {
    return this.items.length === 0;
  }
}
```

The `PriorityQueue` class has a generic type parameter `T` that specifies the type of the elements stored in the queue. It has methods to enqueue, dequeue, peek at the front element, and check if the queue is empty.

To use the `PriorityQueue` class, we can specify the type of the elements it will contain. For example, if we want to create a priority queue of strings:

```typescript
const queue = new PriorityQueue<string>();
queue.enqueue("Low priority", 1);
queue.enqueue("High priority", 10);
queue.enqueue("Medium priority", 5);

console.log(queue.peek().element); // Output: "High priority"
console.log(queue.dequeue().element); // Output: "High priority"
console.log(queue.dequeue().element); // Output: "Medium priority"
console.log(queue.dequeue().element); // Output: "Low priority"
```

We can also use the `PriorityQueue` class with other data types such as numbers or objects:

```typescript
const queue = new PriorityQueue<number>();
queue.enqueue(1, 1);
queue.enqueue(2, 10);
queue.enqueue(3, 5);

console.log(queue.peek().element); // Output: 2
console.log(queue.dequeue().element); // Output: 2
console.log(queue.dequeue().element); // Output: 3
console.log(queue.dequeue().element); // Output: 1

const queue = new PriorityQueue<{ name: string, age: number }>();
queue.enqueue({ name: "John", age: 30 }, 1);
queue.enqueue({ name: "Jane", age: 25 }, 10);
queue.enqueue({ name: "Bob", age: 35 }, 5);

console.log(queue.dequeue().element.name); // Output: "Jane"
console.log(queue.dequeue().element.name); // Output: "Bob"
console.log(queue.dequeue().element.name); // Output: "John"
```

In this example, we created a priority queue of objects and used the `enqueue` method to add elements to the queue based on their priority. We also used the `dequeue` and `peek` methods to remove and view the elements in the queue.

### Simple Cache System

A cache system is a way of storing data in memory so that it can be retrieved quickly.

Here is an example of a generic `Cache` class in TypeScript:

```typescript
class Cache<T> {
  private cache: Map<string, T> = new Map<string, T>();

  set(key: string, value: T) {
    this.cache.set(key, value);
  }

  get(key: string): T | undefined {
    return this.cache.get(key);
  }

  delete(key: string) {
    this.cache.delete(key);
  }

  clear() {
    this.cache.clear();
  }
}
```

The `Cache` class has a generic type parameter `T` that specifies the type of the data stored in the cache. It has methods to set, get, delete, and clear the cache.

To use the `Cache` class, we can specify the type of the data it will contain. For example, if we want to create a cache of numbers:

```typescript
const cache = new Cache<number>();
cache.set("a", 1);
cache.set("b", 2);
cache.set("c", 3);

console.log(cache.get("a")); // Output: 1
console.log(cache.get("b")); // Output: 2
console.log(cache.get("c")); // Output: 3

cache.delete("b");
console.log(cache.get("b")); // Output: undefined

cache.clear();
console.log(cache.get("a")); // Output: undefined
console.log(cache.get("c")); // Output: undefined
```

We can also use the `Cache` class with other data types such as strings or objects:

```typescript
const cache = new Cache<string>();
cache.set("a", "Hello");
cache.set("b", "World");

console.log(cache.get("a")); // Output: "Hello"
console.log(cache.get("b")); // Output: "World"

const cache = new Cache<{ name: string, age: number }>();
cache.set("a", { name: "John", age: 30 });
cache.set("b", { name: "Jane", age: 25 });

console.log(cache.get("a").name); // Output: "John"
console.log(cache.get("b").name); // Output: "Jane"
```

In this example, we created a cache of different data types and used the `set`, `get`, `delete`, and `clear` methods to manipulate the cache.

### Simple Event Emitter

An event emitter is a way of sending and receiving messages or events between different parts of an application.

Here is an example of a generic `EventEmitter` class in TypeScript:

```typescript
class EventEmitter<T> {
  private listeners: { [event: string]: ((data: T) => void)[] } = {};

  on(event: string, listener: (data: T) => void) {
    if (!this.listeners[event]) {
      this.listeners[event] = [];
    }
    this.listeners[event].push(listener);
  }

  off(event: string, listener: (data: T) => void) {
    if (!this.listeners[event]) {
      return;
    }
    const index = this.listeners[event].indexOf(listener);
    if (index !== -1) {
      this.listeners[event].splice(index, 1);
    }
  }

  emit(event: string, data: T) {
    if (!this.listeners[event]) {
      return;
    }
    this.listeners[event].forEach(listener => listener(data));
  }
}
```

The `EventEmitter` class has a generic type parameter `T` that specifies the type of the data sent with the events. It has methods to subscribe to an event, unsubscribe from an event, and emit an event.

To use the `EventEmitter` class, we can specify the type of the data it will contain. For example, if we want to create an event emitter of strings:

```typescript
const emitter = new EventEmitter<string>();

const listener = (data: string) => {
  console.log(data);
};

emitter.on("message", listener);

emitter.emit("message", "Hello"); // Output: "Hello"
emitter.emit("message", "World"); // Output: "World"

emitter.off("message", listener);
emitter.emit("message", "Goodbye"); // No output
```

We can also use the `EventEmitter` class with other data types such as numbers or objects:

```typescript
const emitter = new EventEmitter<number>();

const listener = (data: number) => {
  console.log(data);
};

emitter.on("count", listener);

emitter.emit("count", 1); // Output: 1
emitter.emit("count", 2); // Output: 2

const emitter = new EventEmitter<{ name: string, age: number }>();

const listener = (data: { name: string, age: number }) => {
  console.log(data.name);
};

emitter.on("person", listener);

emitter.emit("person", { name: "John", age: 30 }); // Output: "John"
emitter.emit("person", { name: "Jane", age: 25 }); // Output: "Jane"
```

In this example, we created an event emitter of different data types and used the `on`, `off`, and `emit` methods to subscribe to, unsubscribe from, and emit events.

I hope these examples have helped you understand how to create and use a generic class in TypeScript. Generics allow you to write reusable code that can work with multiple data types, making your code more flexible and efficient.

## **Recap**

To create a generic class in TypeScript:

* Use the `<T>` syntax to define the type parameter.
    
* Use the type parameter in the class body to specify the type of properties and parameters.
    
* To use the generic class, specify the type of the elements it will contain.
    
* You can also use constraints to ensure that the type parameter has certain properties or methods.
    

I hope this article has helped you understand how to create a generic class in TypeScript. If you have any questions or comments, please let me know.