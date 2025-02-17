# S06 NSD Assignment

### Question 1 : What happens if a child class does **not** define a constructor in TypeScript?

A) The child class will throw an error because every class must have a constructor.

B) The child class will automatically use the parent class's constructor.

C) The child class will not be able to create an instance.

D) The child class will inherit the parent class properties but not the constructor.

✅ **Correct Answer:** **B** - The child class will automatically use the parent class's constructor.

### Question 2 : What is the primary purpose of the **Decorator Pattern**?

A) To ensure only one instance of a class exists

B) To allow adding behavior to objects dynamically without modifying their code

C) To provide a global access point for objects

D) To create a complex hierarchy of classes through inheritance

✅ **Correct Answer:** **B** - The **Decorator Pattern** allows adding behavior to objects dynamically without modifying their code.

### **Question :**

Why is calling `super()` necessary in a child class constructor, if present, in TypeScript?

A) It is only required if the parent class has a constructor.

B) It is required to initialize the parent class before accessing `this`.

C) It is only needed when calling a method from the parent class.

D) It is optional and does not affect execution.

✅ **Correct Answer:** **B** - It is required to initialize the parent class before accessing `this`.

### **Question :**

Consider the following code:

```tsx

class Parent {
    constructor(public name: string) {
        console.log("Parent constructor called");
    }
}

class Child extends Parent {}

const obj = new Child("Alice");
```

What will be the output?

A) `"Parent constructor called"`

B) No output, since `Child` does not have a constructor.

C) `"Child constructor called"`

D) TypeScript will throw an error because `super()` is missing in `Child`.

**Correct Answer:** **A** - `"Parent constructor called"` (Because `Child` inherits the `Parent` constructor).

### Question : Which of the following correctly fixes the error in this child class constructor?

```tsx
typescript
CopyEdit
class Parent {
    constructor(public name: string) {}
}

class Child extends Parent {
    constructor(public age: number) {
        console.log("Child constructor called");
    }
}

```

A) Remove the constructor from the `Child` class.

B) Add `super(name)` before `console.log("Child constructor called")`.

C) Call `this.name = "Default";` before calling `super()`.

D) Change `extends Parent` to `implements Parent`.

✅ **Correct Answer:** **B** - Add `super(name)` before `console.log("Child constructor called")`.

## Problem Statement:

Create a `Person` class in TypeScript that represents a person with a name and age.

### Steps:

1. Define a class named `Person`.
2. Add two properties: `name` (string) and `age` (number).
3. Create a constructor that initializes these properties.
4. Instantiate an object of this class and print the details.

### **Submission Guidelines:**

- Submit your Masai Git repository link.

## Problem Statement:

Modify the `Person` class to include getter and setter methods for the `age` property. Ensure that the `age` cannot be set to a negative value.

### Steps:

1. Add a getter method `getAge()` that returns the age.
2. Add a setter method `setAge(value: number)` that updates the age only if the value is non-negative.
3. Try setting a negative age and verify that it does not update the value.

### **Submission Guidelines:**

- Submit your Masai Git repository link.

### Problem Statement :

Create a simple **Observer Pattern** where a **Subject** maintains a list of **Observers**. When the subject's state changes, it notifies all observers.

### **Steps to Implement:**

1. Create an `Observer` interface with a method `update(data: any)`.
2. Create a `Subject` class with methods:
    - `subscribe(observer: Observer)`: Adds an observer.
    - `unsubscribe(observer: Observer)`: Removes an observer.
    - `notify(data: any)`: Notifies all observers.
3. Implement a `ConcreteObserver` class that logs the received updates.
4. Demonstrate the pattern by creating a subject, adding/removing observers, and updating data.

### **Submission Guidelines:**

- Submit your Masai Git repository link.

## **Problem Statement:**

You are given a partially implemented **Weather Monitoring System**, but it has some bugs. Your task is to debug and fix it so that the system correctly notifies all observers when the weather changes.

### **Given Faulty Code:**

```tsx
class WeatherStation {
    observers: Observer[];

    constructor() {
        this.observers = [];
    }

    addObserver(observer) {
        this.observers.push(observer);
    }

    removeObserver(observer) {
        const index = this.observers.indexOf(observer);
        if (index > 0) {
            this.observers.splice(index, 1);
        }
    }

    notifyObservers(temperature) {
        this.observers.forEach(observer => observer.notify(temperature));
    }
}

class WeatherApp {
    constructor(name) {
        this.name = name;
    }

    notify(temp) {
        console.log(`${this.name} received temperature update: ${temp}°C`);
    }
}

const station = new WeatherStation();
const app1 = new WeatherApp("WeatherNow");
const app2 = new WeatherApp("ClimateTracker");

station.addObserver(app1);
station.addObserver(app2);

station.notifyObservers(25); // Expected: Both apps should receive 25°C update
station.removeObserver(app1);
station.notifyObservers(30); // Expected: Only "ClimateTracker" should receive 30°C
```

### **Expected Fixes:**

- Correct the **removeObserver** method (index check issue).
- Ensure all observers receive updates properly.
- Fix any missing TypeScript types.

## Problem Statement : Decorator Pattern

A developer made a mistake in **chaining decorators**, which causes an incorrect cost calculation. Your task is to **identify and fix the bug**.

### **Buggy Code:**

```tsx

class HoneyDecorator extends Beverage {
    beverage: Beverage;
    constructor(beverage: Beverage) {
        super();
        this.beverage = beverage;
    }
    cost(): number {
        return 5;  // BUG: Should add cost to existing beverage cost
    }
}

let myDrink = new Tea();
myDrink = new HoneyDecorator(myDrink);
console.log(`Final Cost: ${myDrink.cost()}`);  // Expected: 15

```

### Hint :

- The decorator should add cost to the **existing** beverage cost, not **override** it.

## **Problem Statement:**

You need to implement a **coffee shop system** where customers can order different beverages and customize them with multiple add-ons.

### **Steps to Solve:**

1. Create three beverages:
    - `Espresso` (cost: `40`).
    - `Latte` (cost: `50`).
    - `Cappuccino` (cost: `45`).
2. Create three decorators:
    - `MilkDecorator` (adds `10`).
    - `VanillaDecorator` (adds `15`).
    - `WhippedCreamDecorator` (adds `12`).
3. Allow users to create **customized beverages** using these decorators.
4. Print the final cost after all customizations.

## Problem Statement : Decorator Pattern

You are building a **text formatting system** where text can be displayed in **bold, italic, or underlined styles** using the **Decorator Pattern**.

### **Steps to Solve:**

1. Create a `Text` interface with the method `render(): string`.
2. Implement `PlainText` class that stores a text string and returns it when `render()` is called.
3. Create an abstract `TextDecorator` class that wraps a `Text` object.
4. Implement three decorators:
    - `BoldDecorator` (adds `<b>` tags around the text).
    - `ItalicDecorator` (adds `<i>` tags around the text).
    - `UnderlineDecorator` (adds `<u>` tags around the text).
5. Test by applying multiple decorators dynamically.