# NSD S06 LLD : Decorator Pattern

## Problem Statement

In a cafe, customers can order various beverages, such as tea and coffee. These beverages can have different combinations of ingredients like honey, sugar, and whipped cream. The system needs to handle the following:

- Each beverage has a description and a cost.
- Beverages can be customised with additional ingredients by applying decorators to the basic beverage.
- The decorators should allow the system to dynamically alter the beverage's cost based on the ingredients added, while still adhering to the object-oriented design principles.

### Objective:

Design a flexible beverage ordering system where:

1. Different types of beverages (e.g., lemon tea, green tea, coffee) can be created with their base cost.
2. The base beverages can be decorated with additional ingredients (e.g., honey, sugar, whipped cream), which will modify their cost dynamically.
3. The system should allow users to retrieve the final cost of a beverage after all customisations.
4. The design should be flexible enough to add more beverages or decorators in the future without modifying the core system.

## Approach

Class 

1. Client Code
2. Beverage Class
3. Child Beverage Classes
4. Decorator Classes

Design Decision : Why Inheritance?

1. Need both description and cost method

Solution : 

- The **Decorator Pattern** is a structural design pattern that allows us to dynamically add new behavior or responsibilities to an object, without altering its core structure. It works by **wrapping** an object (usually referred to as the **decorated object**) with additional functionality through **decorators**.

## Code Solution

```jsx
class Beverage{
    description():string{
        return "Default Description";
    }
    cost(): number{
        return 0;
    }
}

// Design Decision : Interface or Inheritance?

// Developer Tip (Mac) : Command + . to show quick fix
class LemonTea extends Beverage{
    description(): string {
        return "Lemon Tea";
    }

    cost(): number {
        return 10;
    }
}

class GreenTea extends Beverage{
    description(): string {
        return "Green Tea";
    }

    cost(): number {
        return 20;
    }
}

class Coffee extends Beverage{
    description(): string {
        return "Coffee";
    }

    cost(): number {
        return 30;
    }
}

class Espresso extends Beverage{
    description(): string {
        return "Espresso";
    }

    cost(): number {
        return 40;
    }
}

class HoneyDecorator extends Beverage{
    protected beverage: Beverage;

    constructor(beverage:Beverage){
        super();
        this.beverage = beverage;
    }

    cost():number{
        return this.beverage.cost() + 5;
    }
}

class SugarDecorator extends Beverage{
    protected beverage:Beverage;

    constructor(beverage: Beverage){
        super();
        this.beverage = beverage;
    }

    cost(): number {
        return this.beverage.cost() + 2;
    }
}

class WhippedCreamDecorator extends Beverage{
    protected beverage: Beverage;

    constructor(beverage: Beverage){
        super();
        this.beverage = beverage;
    }

    cost(): number {
        return this.beverage.cost() + 10;
    }
}

let myFavTea = new LemonTea();
console.log(`My Beverage is ${myFavTea.description()}`);
// console.log(`new Cost is ${myFavTea.cost()}`);

myFavTea = new HoneyDecorator(myFavTea);
// console.log(`new Cost is ${myFavTea.cost()}`);

myFavTea = new SugarDecorator(myFavTea);
// console.log(`new Cost is ${myFavTea.cost()}`);

myFavTea = new SugarDecorator(myFavTea);
console.log(`My Beverage Cost : ${myFavTea.cost()}`);

let herFavTea = new GreenTea();
console.log(`Her Beverage is ${herFavTea.description()}`);
herFavTea = new HoneyDecorator(herFavTea);
herFavTea = new HoneyDecorator(herFavTea);
console.log(`Her Beverage Cost : ${herFavTea.cost()}`);

let venuBeverage = new LemonTea();
console.log(`Venu's Beverage is  ${venuBeverage.description()}`);
venuBeverage = new HoneyDecorator(venuBeverage);
console.log(`Venu Beverage cost is ${venuBeverage.cost()}`);

let harshitBeverage = new LemonTea();
console.log(`Harshit's Bevarage is ${harshitBeverage.description()}`);
harshitBeverage = new SugarDecorator(harshitBeverage);
harshitBeverage = new SugarDecorator(harshitBeverage);
console.log(`Harshit's Bevarage cost is ${harshitBeverage.cost()}`);

let dinkarBeverage = new Espresso();
dinkarBeverage = new SugarDecorator(dinkarBeverage);
dinkarBeverage = new SugarDecorator(dinkarBeverage);
dinkarBeverage = new WhippedCreamDecorator(dinkarBeverage);
console.log(`Dinkar's Beverage cost is ${dinkarBeverage.cost()}`);
```

## Class Diagram

## Application

1. User authentication system
2. **Document Formatting** – Applying bold, italic, or underline to a text paragraph.
3. **Online Shopping Cart** – Adding discounts, taxes, or shipping fees dynamically.

## Benefit

1. You can extend an object’s behavior without making a new subclass.
2. You can add or remove responsibilities from an object at runtime.
3. You can combine several behaviors by wrapping an object into multiple decorators.
4. *Single Responsibility Principle*. You can divide a monolithic class that implements many possible variants of behavior into several smaller classes.

## Limitation

1. It’s hard to remove a specific wrapper from the wrappers stack.
2. It’s hard to implement a decorator in such a way that its behavior doesn’t depend on the order in the decorators stack.
3. The initial configuration code of layers might look pretty ugly.