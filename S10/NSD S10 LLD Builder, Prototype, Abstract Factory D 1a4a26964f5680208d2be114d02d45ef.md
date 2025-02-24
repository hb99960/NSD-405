# NSD S10 LLD : Builder, Prototype, Abstract Factory Design Patterns

## Builder Design Pattern

The **Builder Pattern** is a **creational design pattern** that **separates object construction from its representation**. It is particularly useful when **objects have multiple optional parameters** or **complex construction logic**.

## **Problem Statement**

Imagine a **restaurant ordering system** where customers can **customize** their meal with different **combinations of main dishes, drinks, and desserts**.

Example:

- A **meal order** might include:
    - **Main Dish**: Pizza
    - **Drink**: Coke
    - **Dessert**: Brownie

```tsx
// Step 1: Define the Product (Meal)
class Meal {
    mainDish: string;
    drink?: string;
    dessert?: string;

    constructor(mainDish: string, drink?: string, dessert?: string) {
        this.mainDish = mainDish;
        this.drink = drink;
        this.dessert = dessert;
    }

    display(): void {
        console.log(`🍽️ Meal: ${this.mainDish}, Drink: ${this.drink ?? "None"}, Dessert: ${this.dessert ?? "None"}`);
    }
}

// Step 2: Create the Builder
class MealBuilder {
    private mainDish: string = "Basic Meal";
    private drink?: string;
    private dessert?: string;

    setMainDish(mainDish: string): this {
        this.mainDish = mainDish;
        return this; // Allows method chaining
    }

    addDrink(drink: string): this {
        this.drink = drink;
        return this;
    }

    addDessert(dessert: string): this {
        this.dessert = dessert;
        return this;
    }

    build(): Meal {
        return new Meal(this.mainDish, this.drink, this.dessert);
    }
}

// Step 3: Usage - Creating Different Meal Orders
const meal1 = new MealBuilder().setMainDish("Burger").addDrink("Coke").build();
meal1.display(); // 🍽️ Meal: Burger, Drink: Coke, Dessert: None

const meal2 = new MealBuilder().setMainDish("Pizza").addDrink("Pepsi").addDessert("Brownie").build();
meal2.display(); // 🍽️ Meal: Pizza, Drink: Pepsi, Dessert: Brownie

const meal3 = new MealBuilder().setMainDish("Pasta").addDessert("Ice Cream").build();
meal3.display(); // 🍽️ Meal: Pasta, Drink: None, Dessert: Ice Cream
```

### Why not use Constructor?

- If we **directly pass parameters to the constructor**, we may run into **constructor overload issues** (also known as the **Telescoping Constructor Problem**).
- If we use a **constructor**, we need multiple constructor overloads (`Meal(main, drink)`, `Meal(main, drink, dessert)`, etc.). Constructor overloads **don't scale well.**
- **Constructors** become difficult to manage with too many optional parameters.
- Customers should **only add the items they want**, instead of passing `null` for optional parameters.
- We need **a flexible way** to customize a meal **without modifying existing code**.

### Why to have separate builder class?

- The **Builder Pattern separates object creation** (`MealBuilder`) from its **representation** (`MEal`), following the **Single Responsibility Principle (SRP)**.
- The `Meal` class **should only represent data**, not handle **complex creation logic**.

### Method Chaining

This code uses **Method Chaining**, an **Object-Oriented Programming (OOP)** concept where multiple methods are called **on the same object sequentially**.

Each method **returns `this` (the current object), t**his allows further method calls **on the same instance**.

### The **Builder Pattern** is useful when:

- **Objects have multiple optional parameters**.
- **Constructors become hard to manage**.
- **Flexible object creation is needed**.

### Builder vs Factory Pattern

| Feature | **Builder Pattern**  | **Factory Pattern**  |
| --- | --- | --- |
| **Use Case** | Creating **complex objects** with **custom configurations**. | Creating **multiple instances of similar objects**. |
| **Object Creation** | Step-by-step **incremental construction**. | **Single-step instantiation** with predefined configurations. |
| **Complexity Handling** | Ideal when an object has **many optional fields**. | Best for creating **different types of objects**. |
| **Method Chaining** | Supports **method chaining** (fluent interface). | Not applicable. |
| **Encapsulation** | Hides **complex construction logic**in the Builder. | Hides **object instantiation logic** in the Factory. |
| **Best Used When** | You need **fine-grained control** over object construction. | You need a **simple way to create new instances** without worrying about instantiation details. |

---

## Prototype Design Pattern

The **Prototype Pattern** is a **creational design pattern** that allows us to **create new objects by copying existing ones (cloning)** rather than constructing them from scratch. This approach **optimizes performance and memory usage**, especially when object creation is expensive.

### Code : Without Prototype Design

```tsx
class Soldier {
    name: string;
    weapon: string;

    constructor(name: string, weapon: string) {
        this.name = name;
        this.weapon = weapon;
        console.log(`New Soldier Created: ${this.name} with ${this.weapon}`);
    }
}

const soldier1 = new Soldier("John", "Rifle");
const soldier2 = new Soldier("Alex", "Rifle"); // New object, expensive!
```

### Code : Prototype Design

```tsx
// Step 1: Define the Prototype Interface
interface Clonable {
    clone(): this;
}

// Step 2: Create a Concrete Prototype
class Soldier implements Clonable {
    name: string;
    weapon: string;

    constructor(name: string, weapon: string) {
        this.name = name;
        this.weapon = weapon;
    }

    clone(): this {
        return Object.assign(Object.create(Object.getPrototypeOf(this)), this);
    }

    display(): void {
        console.log(`Soldier: ${this.name}, Weapon: ${this.weapon}`);
    }
}

// Step 3: Usage
const soldier1 = new Soldier("John", "Rifle");
const soldier2 = soldier1.clone(); // Cloning instead of creating a new object

soldier2.name = "Alex"; // Modifying only the cloned object

soldier1.display(); // Soldier: John, Weapon: Rifle
soldier2.display(); // Soldier: Alex, Weapon: Rifle
```

### Prototype Interface

- `this` ensures that **each subclass implements cloning correctly**.
- If `clone()` is called on a `Soldier`, it will return **another `Soldier`**.
- **`Object.getPrototypeOf(this)`** → Gets the prototype of the current object (`this`).
- **`Object.create(...)`** → Creates a **new empty object** with the same prototype.
- **`Object.assign(..., this)`** → Copies all properties from `this` to the new object.

### Note

- `clone()` creates a **new object** but does **not call the constructor again**.
- This ensures **faster object creation** compared to calling `new Soldier(...)` again.
- Clones reuse memory efficiently.
- Use Prototype pattern when objects are duplicate, Use Constructor when objects are unique

## Abstract Factory Design Pattern

The **Abstract Factory Pattern** is a **creational design pattern** that provides an **interface for creating families of related objects** without specifying their concrete classes.

```tsx
// Step 1: Define an interface for Product
interface Product {
  name: string;
  price: number;
  getDescription(): string;
}

// Step 2: Implement Apple Products
class AppleLaptop implements Product {
  constructor(public name: string, public price: number) {}
  getDescription(): string {
    return `🍏 Apple Laptop: ${this.name}, Price: $${this.price}`;
  }
}

class AppleMobile implements Product {
  constructor(public name: string, public price: number) {}
  getDescription(): string {
    return `🍏 Apple Mobile: ${this.name}, Price: $${this.price}`;
  }
}

// Step 3: Implement Samsung Products
class SamsungLaptop implements Product {
  constructor(public name: string, public price: number) {}
  getDescription(): string {
    return `📱 Samsung Laptop: ${this.name}, Price: $${this.price}`;
  }
}

class SamsungMobile implements Product {
  constructor(public name: string, public price: number) {}
  getDescription(): string {
    return `📱 Samsung Mobile: ${this.name}, Price: $${this.price}`;
  }
}

// Step 4: Define an Abstract Factory Interface
interface ProductFactory {
  createLaptop(): Product;
  createMobile(): Product;
}

// Step 5: Implement Concrete Factories
class AppleFactory implements ProductFactory {
  createLaptop(): Product {
    return new AppleLaptop("MacBook Pro", 2500);
  }

  createMobile(): Product {
    return new AppleMobile("iPhone 15", 1200);
  }
}

class SamsungFactory implements ProductFactory {
  createLaptop(): Product {
    return new SamsungLaptop("Galaxy Book", 1800);
  }

  createMobile(): Product {
    return new SamsungMobile("Galaxy S23", 1000);
  }
}

// Step 6: Factory Selector
class FactoryProvider {
  static getFactory(brand: string): ProductFactory {
    if (brand === "Apple") {
      return new AppleFactory();
    } else if (brand === "Samsung") {
      return new SamsungFactory();
    } else {
      throw new Error("Invalid brand type");
    }
  }
}

// Step 7: Client Code
const appleFactory = FactoryProvider.getFactory("Apple");
const myAppleLaptop = appleFactory.createLaptop();
const myAppleMobile = appleFactory.createMobile();

console.log(myAppleLaptop.getDescription()); // 🍏 Apple Laptop: MacBook Pro, Price: $2500
console.log(myAppleMobile.getDescription()); // 🍏 Apple Mobile: iPhone 15, Price: $1200

const samsungFactory = FactoryProvider.getFactory("Samsung");
const mySamsungLaptop = samsungFactory.createLaptop();
const mySamsungMobile = samsungFactory.createMobile();

console.log(mySamsungLaptop.getDescription()); // 📱 Samsung Laptop: Galaxy Book, Price: $1800
console.log(mySamsungMobile.getDescription()); // 📱 Samsung Mobile: Galaxy S23, Price: $1000

```

## **Differences Between Abstract Factory & Other Factory Patterns**

| **Feature** | **Factory Method** | **Abstract Factory** |
| --- | --- | --- |
| **Creates a single object?** | ✅ Yes | ❌ No (Creates related objects) |
| **Creates families of objects?** | ❌ No | ✅ Yes |
| **Encapsulates object creation?** | ✅ Yes | ✅ Yes |
| **Example Use Case** | **Create a single product** (e.g., `new Laptop()`) | **Create an entire family** (e.g., **AppleFactory** → Laptop & Mobile) |

**Key Features:**

- **Encapsulates multiple factory methods** inside a higher-level factory.
- Ensures that **created objects belong to the same family**.
- Supports **scalability** by adding new product families without modifying existing code.
- **Hides object creation complexity** from the client.

---

## **Summary of Patterns**

| **Pattern** | **Problem Solved** | **Key Concept** |
| --- | --- | --- |
| **Builder** ✅ | Constructing complex objects step-by-step | Provides a **flexible object creation** mechanism |
| **Prototype** ✅ | Creating **multiple copies** of an object efficiently | Clones existing objects instead of creating new ones |
| **Abstract Factory**✅ | Creating **related objects dynamically** | Defines an interface for creating **families of objects** |

## **🎯 Conclusion**

✅ **Builder Pattern** helps in constructing **complex objects with different configurations**.

✅ **Prototype Pattern** helps in **cloning objects efficiently** to reduce memory usage.

✅ **Abstract Factory Pattern** helps in **creating related objects dynamically without modifying existing code**.