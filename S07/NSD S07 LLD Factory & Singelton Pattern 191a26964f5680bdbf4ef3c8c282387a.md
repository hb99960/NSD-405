# NSD S07 LLD : Factory & Singelton Pattern

## Problem Statement - Factory Pattern

## Approach

If you see object creation without new keyword, but with factory function (though object creation inside the factory function is done using new keyword)

The Factory Pattern is a creational design pattern that provides an interface for creating objects without specifying their concrete classes. Instead of using the `new` keyword directly in the client code, object creation is encapsulated within a factory function. Inside this function, objects are still instantiated using `new`, but this detail is hidden from the user.

### **Key Characteristics of the Factory Pattern:**

- The factory function abstracts the object creation logic.
- The `new` keyword is used **inside** the factory function, but **not in the client code**.
- Useful for managing object creation complexity, particularly when dealing with multiple subclasses or dynamic object creation.

This pattern is especially useful when dealing with complex object creation logic, where direct instantiation might introduce unnecessary dependencies.

### **Example Approach:**

Instead of:

```tsx
const product = new Product("Laptop", 1000);

```

With a factory function:

```tsx
const product = ProductFactory.createProduct("Laptop", 1000);

```

## Code

```tsx
// Step 1: Define an interface for the product
interface Product {
  name: string;
  price: number;
  getDescription(): string;
}

// Step 2: Implement concrete product classes
class Laptop implements Product {
  constructor(public name: string, public price: number) {}
  getDescription(): string {
    return `Laptop: ${this.name}, Price: $${this.price}`;
  }
}

class Mobile implements Product {
  constructor(public name: string, public price: number) {}
  getDescription(): string {
    return `Mobile: ${this.name}, Price: $${this.price}`;
  }
}

// Step 3: Create a Factory class
class ProductFactory {
  static createProduct(type: string, name: string, price: number): Product {
    if (type === "Laptop") {
      return new Laptop(name, price);
    } else if (type === "Mobile") {
      return new Mobile(name, price);
    } else {
      throw new Error("Invalid product type");
    }
  }
}

// Step 4: Use the Factory to create objects
const myLaptop = ProductFactory.createProduct("Laptop", "MacBook Pro", 2500);
const myMobile = ProductFactory.createProduct("Mobile", "iPhone 15", 1200);

console.log(myLaptop.getDescription()); // Laptop: MacBook Pro, Price: $2500
console.log(myMobile.getDescription()); // Mobile: iPhone 15, Price: $1200
```

## Class Diagram

```tsx
        Product (Interface)
             ↑
    --------------------
    |                  |
 Laptop            Mobile
    ↑                  ↑
    |                  |
   ProductFactory (Creates Objects)

```

- `Product` is an interface that defines the common behavior for all product types.
- `Laptop` and `Mobile` are concrete implementations of `Product`.
- `ProductFactory` is responsible for creating instances of `Laptop` or `Mobile` based on the given type.

## When to use?

The Factory Pattern is useful in the following situations:

1. **Object creation logic is complex**
    - When object initialization involves multiple steps or dependencies.
2. **You want to manage different object types dynamically**
    - When multiple subclasses share a common interface but the exact type is decided at runtime.
3. **Encapsulation of `new` keyword usage**
    - Avoids exposing the instantiation details to client code.
4. **Promotes maintainability and scalability**
    - Easy to introduce new product types without modifying client code.

## Application

The Factory Pattern is widely used in various real-world applications, including:

1. **UI Component Libraries**
    - Creating UI components dynamically based on configurations.
2. **Database Connection Management**
    - Factory pattern helps in managing different database drivers.
3. **Logging Frameworks**
    - Dynamically create loggers for different outputs (console, file, remote server).
4. **Game Development**
    - Generating different game objects (e.g., enemies, power-ups) dynamically

## Limitation

- **Increased Complexity**
    - Introducing a factory class adds an extra layer of abstraction.
- **May Violate Open/Closed Principle**
    - If new product types require modifying the factory, it can become less extensible.
- **Debugging Can Be Harder**
    - Since the `new` keyword is abstracted, it might be difficult to trace object instantiation in large applications.

## Benefits

✅ **Encapsulation** - The client code doesn’t need to worry about the instantiation logic.

✅ **Code Reusability** - Centralized object creation logic promotes reusability.

✅ **Scalability** - Easy to introduce new object types without modifying existing code.

✅ **Decoupling** - Client code is decoupled from concrete class implementations.

---

## Problem Statement - Singleton Pattern

- **Singleton is a creational design pattern, which ensures that only one object of its kind exists and provides a single point of access to it for any other code.**
- The **Singleton Pattern** ensures that a class has **only one instance** and provides a **global access point** to it.
- Used when a **single shared resource** (e.g., database connection, configuration settings, logging service) is required across an application.
- Prevents multiple instantiations that could lead to **unnecessary resource consumption** or **inconsistent state management**.

The Singleton pattern solves two problems at the same time, violating the *Single Responsibility Principle*:

1. **Ensure that a class has just a single instance**.
2. **Provide a global access point to that instance**.

## Approach

## Code

```tsx
class Singleton {
    static #instance: Singleton;
    
    private constructor() { }
   
    public static get instance(): Singleton {
        if (!Singleton.#instance) {
            Singleton.#instance = new Singleton();
        }
        return Singleton.#instance;
    }
    
    public someBusinessLogic() {
        // ...
    }
}

const s1 = Singleton.instance;
const s2 = Singleton.instance;

if (s1 === s2) {
    console.log(
        'Singleton works, both variables contain the same instance.'
    );
} else {
    console.log('Singleton failed, variables contain different instances.');
}
```

### Getters in Typescript

```tsx
//In TypeScript, getter methods do not require parentheses when accessed. This means that when you define a getter like this:
class Example {
    private value: number = 42;

    get data(): number {
        return this.value;
    }
}

const obj = new Example();
console.log(obj.data); // ✅ No parentheses needed
```

- Getter methods are accessed like properties, without parentheses, as shown in the code.

## Class Diagram

## When to use?

**Use the Singleton pattern when a class in your program should have just a single instance available to all clients; for example, a single database object shared by different parts of the program.**

The Singleton pattern disables all other means of creating objects of a class except for the special creation method. This method either creates a new object or returns an existing one if it has already been created.

## Application

1. Logger
2. Database Connection

## Benefits

- You can be sure that a class has only a single instance.
- You gain a global access point to that instance.
- The singleton object is initialized only when it’s requested for the first time.

### Limitations

- Violates the *Single Responsibility Principle*. The pattern solves two problems at the time.
- The Singleton pattern can mask bad design, for instance, when the components of the program know too much about each other.
- The pattern requires special treatment in a multithreaded environment so that multiple threads won’t create a singleton object several times.
- It may be difficult to unit test the client code of the Singleton because many test frameworks rely on inheritance when producing mock objects. Since the constructor of the singleton class is private and overriding static methods is impossible in most languages, you will need to think of a creative way to mock the singleton. Or just don’t write the tests. Or don’t use the Singleton pattern.