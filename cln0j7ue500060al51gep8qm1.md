---
title: "Another Deep Dive into TypeScript's Design Patterns and more..."
seoDescription: "Design patterns are reusable solutions to common software design problems. They provide proven approaches to structuring code, improving maintainability..."
datePublished: Tue Sep 26 2023 16:28:38 GMT+0000 (Coordinated Universal Time)
cuid: cln0j7ue500060al51gep8qm1
slug: another-deep-dive-into-typescripts-design-patterns-and-more
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1695745600022/2fa1c61c-a031-4207-8920-961f6978c443.png
tags: javascript, web-development, reactjs, typescript, nextjs

---

Design patterns are reusable solutions to common software design problems. They provide proven approaches to structuring code, improving maintainability, and promoting code reusability. In this chapter, we'll explore several design patterns commonly used in TypeScript projects, along with practical examples and guidance on when to apply them.

## Creational Patterns

### Singleton Pattern

**Explanation:** The Singleton pattern ensures that a class has only one instance and provides a global point of access to that instance. It's used when you want to limit the instantiation of a class to a single instance and provide a way to access that instance from anywhere in your application.

**Example:**

```typescript
class Singleton {
  private static instance: Singleton;

  private constructor() {} // Private constructor prevents direct instantiation

  static getInstance(): Singleton {
    if (!Singleton.instance) {
      Singleton.instance = new Singleton();
    }
    return Singleton.instance;
  }
}
```

In this code example, the `Singleton` class has a private static `instance` variable and a private constructor to prevent direct instantiation. The `getInstance` method is used to get the singleton instance, creating it if it doesn't exist.

### Factory Method Pattern

**Explanation:** The Factory Method pattern defines an interface for creating objects but allows subclasses to alter the type of objects that will be created. It's beneficial when you want to provide a common interface for creating objects while allowing subclasses to decide which class to instantiate.

**Example:**

```typescript
interface Product {
  operation(): string;
}

class ConcreteProductA implements Product {
  operation(): string {
    return 'Product A';
  }
}

class ConcreteProductB implements Product {
  operation(): string {
    return 'Product B';
  }
}

abstract class Creator {
  abstract factoryMethod(): Product;

  someOperation(): string {
    const product = this.factoryMethod();
    return `Creator: ${product.operation()}`;
  }
}
```

In this code example, we have a `Product` interface and concrete implementations (`ConcreteProductA` and `ConcreteProductB`). The `Creator` class declares a factory method `factoryMethod` that returns a `Product` instance. Subclasses (e.g., `ConcreteCreatorA` and `ConcreteCreatorB`) implements this factory method to create specific product instances. Clients can use the creator to create products without knowing their concrete types.

### Builder Pattern

**Explanation:** The Builder pattern separates the construction of a complex object from its representation, allowing you to create different variations of an object using the same construction process. It's useful when an object has a complex initialization process with many optional parameters.

**Example:**

```typescript
class Product {
  parts: string[] = [];

  addPart(part: string): void {
    this.parts.push(part);
  }

  showParts(): void {
    console.log(`Product parts: ${this.parts.join(', ')}`);
  }
}

interface Builder {
  buildPart1(): void;
  buildPart2(): void;
  getResult(): Product;
}

class ConcreteBuilder implements Builder {
  private product: Product = new Product();

  buildPart1(): void {
    this.product.addPart('Part 1');
  }

  buildPart2(): void {
    this.product.addPart('Part 2');
  }

  getResult(): Product {
    return this.product;
  }
}
```

In this code example, the `Product` class represents a complex object. The `ConcreteBuilder` class implements the `Builder` interface to construct the product. The builder allows you to add parts step by step and eventually retrieve the fully constructed product.

## Structural Patterns

### Decorator Pattern

**Explanation:** The Decorator pattern allows you to add behavior or responsibilities to objects dynamically without altering their class. It's useful when you need to extend the functionality of objects in a flexible and reusable way.

**Example:**

```typescript
interface Coffee {
  cost(): number;
  description(): string;
}

class SimpleCoffee implements Coffee {
  cost(): number {
    return 5;
  }

  description(): string {
    return 'Simple Coffee';
  }
}
```

In this code example, the `SimpleCoffee` class implements the `Coffee` interface. It represents a basic coffee. The Decorator pattern allows you to add decorators like `MilkDecorator` to enhance the coffee's functionality without modifying the `SimpleCoffee` class.

### Adapter Pattern

**Explanation:** The Adapter pattern allows objects with incompatible interfaces to work together by providing a wrapper (adapter) that translates one interface into another. It's used to make existing classes work with others without modifying their source code.

**Example:**

```typescript
class OldSystem {
  doLegacyStuff(): string {
    return 'Legacy functionality';
  }
}

interface NewSystem {
  doNewStuff(): string;
}

class Adapter implements NewSystem {
  private oldSystem: OldSystem;

  constructor(oldSystem: OldSystem) {
    this.oldSystem = oldSystem;
  }

  doNewStuff(): string {
    return `Adapter: ${this.oldSystem.doLegacyStuff()}`;
  }
}
```

In this code example, the `OldSystem` class has a legacy interface and the `Adapter` class implements the `NewSystem` interface. It wraps the `OldSystem` translates its legacy functionality into a new interface, allowing them to work together seamlessly.

### Composite Pattern

**Explanation:** The Composite pattern lets you compose objects into tree structures to represent part-whole hierarchies. It allows clients to treat individual objects and compositions of objects uniformly. This is useful when you want to work with hierarchies of objects, whether they are individual objects or groups of objects.

**Example:**

```typescript
abstract class Component {
  abstract operation(): string;
}

class Leaf extends Component {
  operation(): string {
    return 'Leaf';
  }
}

class Composite extends Component {
  private children: Component[] = [];

  add(child: Component): void {
    this.children.push(child);
  }

  operation(): string {
    return `Composite [${this.children.map((child) => child.operation()).join(', ')}]`;
  }
}
```

In this code example, the `Component` class represents both leaf nodes (individual objects) and composite nodes (groups of objects). The `Composite` class can contain child components, and its `operation` method can work with both leaf and composite components uniformly.

## Behavioral Patterns

### Observer Pattern

**Explanation:** The Observer pattern defines a one-to-many dependency between objects, where one object (the subject) maintains a list of its dependents (observers) and notifies them of state changes. It's used when one object needs to notify multiple others about changes without knowing who or what those observers are.

**Example:**

```typescript
interface Observer {
  update(message: string): void;
}

class ConcreteObserver implements Observer {
  constructor(private name: string) {}

  update(message: string): void {
    console.log(`${this.name} received message: ${message}`);
  }
}

class Subject {
  private observers: Observer[] = [];

  addObserver(observer: Observer): void {
    this.observers.push(observer);
  }

  removeObserver(observer: Observer): void {
    const index = this.observers.indexOf(observer);
    if (index !== -1) {
      this.observers.splice(index, 1);
    }
  }

  notify(message: string): void {
    for (const observer of this.observers) {
      observer.update(message);

    }
  }
}
```

In this code example, the `Subject` maintains a list of observers (implementing the `Observer` interface) and notifies them of changes. Observers like `ConcreteObserver` can subscribe to the subject and receive notifications when state changes occur.

### Strategy Pattern

**Explanation:** The Strategy pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. It allows the algorithm to vary independently from clients that use it. This is useful when you want to define a set of algorithms and make them easily interchangeable without changing the client code.

**Example:**

```typescript
interface PaymentStrategy {
  pay(amount: number): void;
}

class CreditCardPayment implements PaymentStrategy {
  pay(amount: number): void {
    console.log(`Paid $${amount} via Credit Card.`);
  }
}

class PayPalPayment implements PaymentStrategy {
  pay(amount: number): void {
    console.log(`Paid $${amount} via PayPal.`);
  }
}
```

In this code example, we have payment strategies (`CreditCardPayment` and `PayPalPayment`) that implement the `PaymentStrategy` interface. The `ShoppingCart` class can be configured with different payment strategies, allowing clients to switch between payment methods easily.

### Command Pattern

**Explanation:** The Command pattern encapsulates a request as an object, thereby allowing for parameterization of clients with queues, requests, and operations. It's used when you want to decouple the sender and receiver of a request and allow clients to issue requests without knowing the specific request or receiver.

**Example:**

```typescript
interface Command {
  execute(): void;
}

class Light {
  turnOn(): void {
    console.log('Light is ON');
  }

  turnOff(): void {
    console.log('Light is OFF');
  }
}

class LightOnCommand implements Command {
  constructor(private light: Light) {}

  execute(): void {
    this.light.turnOn();
  }
}

class LightOffCommand implements Command {
  constructor(private light: Light) {}

  execute(): void {
    this.light.turnOff();
  }
}
```

In this code example, the `Command` interface defines the `execute` method, and concrete command classes (`LightOnCommand` and `LightOffCommand`) encapsulates requests to turn a light on and off. A `RemoteControl` class can be configured with different commands, allowing clients to issue commands without knowing the details of how the commands are executed.

## **When to Use Design Patterns**

Design patterns should be used judiciously. They are valuable tools for solving specific types of problems but can also introduce unnecessary complexity if applied indiscriminately. We'll discuss the situations in which each design pattern is most appropriate and when it's best to avoid them.

## Use TypeScript Compiler Flags

TypeScript provides various compiler flags that can improve code quality and help catch potential issues. Here are a few useful flags:

* **\-strict**: Enables strict type checking, which helps catch more type-related errors.
    
* **\-noImplicitAny**: Flags variables and parameters with an implicit "any" type, encouraging you to provide explicit types.
    
* **\-strictNullChecks**: Ensures that variables aren't assigned a value of `null` or `undefined` unless explicitly allowed.
    
* **\-noUnusedLocals**: Flags unused local variables and parameters, helping you identify and remove dead code.
    
* **\-noUnusedParameters**: Flags unused function parameters, promoting clean and efficient code.
    

To use these flags, add them to your `tsconfig.json` or use them directly when invoking the TypeScript compiler.

## Type Assertions vs. Type Casting

While both type assertions (using `as` or `<Type>`) and type casting (using `as Type`) can be used to assert a type, it's often recommended to use type assertions when you're certain about the type and type casting when dealing with potentially undefined values. Type casting with `as` will throw a runtime error if the assertion is incorrect, providing better safety.

```tsx
const value: unknown = "Hello, TypeScript!";
const length1 = (value as string).length; // Safe if value is a string.
const length2 = (value as number).toFixed(2); // Throws a runtime error if value is not a number.
```

## Avoid the `any` Type

TypeScript's `any` type should be used sparingly. Whenever possible, provide explicit types for variables, function parameters, and return values. The `any` type weakens TypeScript's type checking and should only be used when you have no other option or when transitioning existing JavaScript code to TypeScript.

## Use Union Types for Flexibility

Union types allow you to specify that a value can have one of several types. They are useful when a variable can take on different types of values.

```tsx
let result: string | number;
result = "Success";
result = 42;
```

Using union types provides flexibility while maintaining type safety.

## Leverage Intersection Types

Intersection types allow you to combine multiple types into a single type. This is particularly useful when dealing with complex object shapes.

```tsx
type Person = { name: string };
type Address = { address: string };
type Contact = Person & Address;
```

The `Contact` type is a combination of `Person` and `Address`, allowing you to work with objects that have properties from both types.

## Type Guards for Discriminated Unions

When working with discriminated unions, use type guards to narrow down the possible types of a variable based on a common property.

```tsx
type Shape = { kind: "circle"; radius: number } | { kind: "rectangle"; width: number; height: number };

function area(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "rectangle":
      return shape.width * shape.height;
  }
}
```

The `kind` property serves as a discriminator, and the `switch` statement acts as a type guard.

## Explore Utility Types

TypeScript provides utility types in the standard library, such as `Partial`, `Required`, and `Pick`, which can simplify common type transformations. Explore and use these utility types to save time and write more concise code.

```tsx
type PartialPerson = Partial<Person>;
type RequiredPerson = Required<Person>;
type AddressOnly = Pick<Contact, "address">;
```

## Avoid Manual Type Assertions

While type assertions can be useful, avoid using them excessively, especially when a better type inference approach exists. Manual type assertions can bypass type checking, potentially leading to runtime errors.

## Stay Up-to-Date with TypeScript

TypeScript evolves continuously, and new features and improvements are added with each release. Stay up-to-date with TypeScript by checking the official documentation and release notes regularly.

## Experiment and Learn

Experimentation is a great way to learn TypeScript. Create small projects or code snippets to explore TypeScript features and test various patterns and techniques. Learning by doing is often the most effective way to master a language.

## Conclusion

These additional tips and tricks can help you become a more proficient TypeScript developer. By using TypeScript's features effectively, you can write cleaner, more maintainable, and safer code while taking full advantage of the language's capabilities. Continuously improving your TypeScript skills will enable you to tackle a wide range of projects with confidence.