# How to Create a Generic Method in TypeScript?

In TypeScript, a generic method is a type of function that can work with multiple data types. This is useful when we want to create a function that can accept arguments of various types, or when we want to return a value that can be of different types.

To create a generic method in TypeScript, we need to use the `<T>` syntax, where `T` is a placeholder for the type that will be specified later. We can then use `T` throughout the method as a placeholder for the actual data type.

## **Syntax for Creating a Generic Method**

Here is the syntax for creating a generic method in TypeScript:

```typescript
function methodName<T>(arg1: T): T {
  // function body
}
```

In this syntax:

* `function` keyword is used to define a function.
    
* `methodName` is the name of the method.
    
* `<T>` is the generic type parameter, which can be used as a placeholder for the actual data type.
    
* `arg1: T` is the argument of the method, which has a type of `T`.
    
* `: T` is the return type of the method, which is also of type `T`
    

## **Examples of Generic Methods**

Here are some examples of generic methods in TypeScript:

### **Example 1: A Generic Function for Reversing an Array**

In this example, we will create a generic function that can reverse an array of any data type.

```typescript
function reverseArray<T>(arr: T[]): T[] {
  return arr.reverse();
}
```

In this example:

* `reverseArray` is the name of the method.
    
* `<T>` is the generic type parameter, which can be used as a placeholder for the actual data type of the array.
    
* `arr: T[]` is the argument of the method, which is an array of type `T`.
    
* `: T[]` is the return type of the method, which is also an array of type T.
    
    ### **Example 2: A Generic Function for Finding the Maximum Element in an Array**
    
    In this example, we will create a generic function that can find the maximum element in an array of any data type that can be compared using the `>` operator.
    
    ```typescript
    function findMax<T>(arr: T[]): T {
      let max = arr[0];
      for (let i = 1; i < arr.length; i++) {
        if (arr[i] > max) {
          max = arr[i];
        }
      }
      return max;
    }
    ```
    
    In this example:
    
* `findMax` is the name of the method.
    
* `<T>` is the generic type parameter, which can be used as a placeholder for the actual data type of the array.
    
* `arr: T[]` is the argument of the method, which is an array of type **T**  
    `: T` is the return type of the method, which is also of type `T`.
    

We can then use this function like this:

```typescript
let numbers = [1, 2, 3];
let maxNumber = findMax(numbers); // 3

let strings = ['a', 'b', 'c'];
let maxString = findMax(strings); // 'c'
```

### **Example 3: A Generic Function for Creating an Object with Default Values**

* In this example, we will create a generic function that can create an object with default values for a specific data type.
    
    ```typescript
    function createObjectWithDefaults<T>(defaultValue: T): {[key: string]: T} {
      return {
        default: defaultValue
      };
    }
    ```
    
    In this example:
    
    * `createObjectWithDefaults` is the name of the method.
        
    * `<T>` is the generic type parameter, which can be used as a placeholder for the actual data type of the default value.
        
    * `defaultValue: T` is the argument of the method, which is of type `T`.
        
    * `: {[key: string]: T}` is the return type of the method, which is an object with a string key and a value of type `T`.
        

We can then use this function like this:

```typescript
typescript let defaultNumber = createObjectWithDefaults(0); // { default: 0 }

let defaultString = createObjectWithDefaults('Hello'); // { default: 'Hello'}
```

## Constraints

In TypeScript, we can use constraints to specify the types that a generic type parameter can be. This is useful when we want to restrict the types that a generic type parameter can accept or when we want to use certain methods or properties of a type within the generic method.

To specify constraints, we use the `extends` keyword followed by the type that we want to use as a constraint. For example:

```typescript
function methodName<T extends ConstraintType>(arg1: T): T {
  // function body
}
```

In this syntax:

* `T extends ConstraintType` is the constraint that specifies that the generic type parameter `T` must be a subtype of `ConstraintType`.
    

## **Examples of Constraints in Generic Methods**

Here are some examples of how we can use constraints in generic methods:

### **Example 1: A Generic Function for Checking if an Element is in an Array**

In this example, we will create a generic function that can check if an element is in an array. We will use a constraint to specify that the element and the array must be of the same type.

```typescript
function includes<T>(arr: T[], element: T): boolean {
  return arr.includes(element);
}
```

In this example:

* `includes` is the name of the method.
    
* `<T>` is the generic type parameter, which can be used as a placeholder for the actual data type of the array and element.
    
* `arr: T[]` is the first argument of the method, which is an array of type `T`.
    
* `element: T` is the second argument of the method, which is of type `T`.
    
* `: boolean` is the return type of the method, which is a boolean value indicating whether the element is in the array or not.
    

We can then use this function like this:

```typescript
let numbers = [1, 2, 3];
let includesNumber = includes(numbers, 2); // true
let doesNotIncludeNumber = includes(numbers, 4); // false

let strings = ['a', 'b', 'c'];
let includesString = includes(strings, 'b'); // true
let doesNotIncludeString = includes(strings, 'd'); // false
```

### **Example 2: A Generic Function for Cloning an Object**

In this example, we will create a generic function that can clone an object. We will use a constraint to specify that the object must have a `clone()` method that returns a copy of the object.

```typescript
function clone<T extends { clone(): T }>(object: T): T {
  return object.clone();
}
```

In this example:

* `clone` is the name of the method.
    
* `<T extends { clone(): T }>` is the generic type parameter and constraint, which specifies that `T` must have a `clone()` method that returns a value of type `T`.
    
* `object: T` is the argument of the method, which is an object of type `T`.
    
* `: T` is the return type of the method, which is a copy of the object of type `T`.
    

We can then use this function like this:

```typescript
class Point {
  x: number;
  y: number;

  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}
```

We can then use this function like this:

```typescript
let point1 = new Point(1, 2);
let point2 = clone(point1); // point2 is a copy of point1
```

## **Conclusion**

In this article, we learned about generic methods in TypeScript and how to create them using the `<T>` syntax. We also saw some examples of generic methods, such as a function for reversing an array, finding the maximum element in an array, and creating an object with default values. Generic methods are a useful feature of TypeScript that allow us to create functions that can work with multiple data types.