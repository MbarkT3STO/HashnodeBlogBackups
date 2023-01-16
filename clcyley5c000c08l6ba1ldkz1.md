# The Inheritance in TypeScript

TypeScript, a typed superset of JavaScript, allows for object-oriented programming concepts such as classes and interfaces. One of these concepts is inheritance, which allows for one class to inherit the properties and methods of another class. This can help to reduce code duplication and improve code organization.

## **How to Implement Inheritance**

To implement inheritance, the `extends` keyword is used to indicate that one class should inherit from another. For example, consider the following class hierarchy:

```csharp
class Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a sound.`);
  }
}

class Dog extends Animal {
  breed: string;

  constructor(name: string, breed: string) {
    super(name);
    this.breed = breed;
  }

  speak() {
    console.log(`${this.name} barks.`);
  }
}
```

In this example, the `Dog` class inherits from the `Animal` class. The `Dog` class has access to the `name` property and the `speak()` method from the `Animal` class, and can also define its own properties and methods.

The `super()` method is used to call the parent class's constructor, and is always called before the child class's constructor.

## **Overriding Methods**

A child class can also override methods from the parent class. In the example above, the `speak()` method in the `Dog` class overrides the `speak()` method in the `Animal` class. This allows for the child class to have its own implementation of the method, while still maintaining access to the parent class's implementation.

```csharp
let dog = new Dog("Fido", "Golden Retriever");
dog.speak(); 
// Output: Fido barks.
```

## **Access Modifiers**

TypeScript also supports access modifiers, such as `public`, `private`, and `protected`, which can be used to control access to properties and methods within a class and its subclasses. For example, if a property is marked as `private`, it can only be accessed within the class itself.

```csharp
class Animal {
  private age: number;

  constructor(age: number) {
    this.age = age;
  }

  getAge() {
    return this.age;
  }
}

class Dog extends Animal {
  private weight: number;

  constructor(age: number, weight: number) {
    super(age);
    this.weight = weight;
  }

  getWeight() {
    return this.weight;
  }
}

let dog = new Dog(5, 30);
console.log(dog.getAge()); // Error: age is private
console.log(dog.getWeight()); // Output: 30
```

## **Conclusion**

Inheritance is a powerful feature of TypeScript that allows for code reuse and organization. By using the `extends` keyword, a child class can inherit properties and methods from a parent class, while also being able to define its own properties and methods, and override methods from the parent class. Access modifiers can also be used to control access to properties and methods within a class and its sub