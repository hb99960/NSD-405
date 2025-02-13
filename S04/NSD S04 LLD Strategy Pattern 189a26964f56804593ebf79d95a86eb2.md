# NSD S04 LLD : Strategy Pattern

| LOs & Pedagogy | Pitfalls |
| --- | --- |
| Design#1 : Strategy Design Pattern
- First revise the Polymorphism 
- Example : Payment Processing example (Ref Book : Head first Design)
- Explain why we study Designs : Assuming the product will change every 3 months to be updated in the market.
- Discuss why to choose Inheritance or Interface or Abstract class
- Goal is we need more flexibility
- Discuss UML diagram
- Discuss the code (not the live)
- Application : Elevator Design pattern |  |
| Design#2 : Observer Pattern
- Give Simple example of Observer pattern
- Explain Publisher and Subscriber, One-to-many relationship
- Applications of Observer Pattern |  |
|  |  |

## Problem Statement

You are building an e-commerce platform, and your client is requesting a flexible payment gateway system to handle various types of transactions. Initially, the platform only needed a simple **Credit Card Payment** integration. However, as the platform grew and expanded globally, the client requested the ability to handle **multiple payment methods** to cater to international customers and improve customer satisfaction.

First, you implemented a **Credit Card Payment** system, which worked perfectly. But soon after, you added **PayPal** as a new payment option, and soon after that, the team requested the addition of **Stripe**, **Bitcoin**, and **Apple Pay**.

With each new payment system added, the code grew larger, and soon your `PaymentProcessor` class had grown to a large, cumbersome block of code. Every time a new payment method was added, the class became harder to maintain, and the chance of introducing bugs increased with each change. The code for processing payments became bloated with conditions and logic for each payment method.

As the team grew, it became evident that the **PaymentProcessor** class was becoming a bottleneck in the development process. Every time a team member added a new payment option, they had to change the same class, leading to frequent merge conflicts and long review cycles.

The flexibility to add new payment gateways was severely limited, and the growing class was difficult to understand and modify without introducing errors.

## Approach :

Understand the core demand 

1. The platform must process payments using different payment methods (e.g., **Credit Card**, **PayPal**, **Stripe**, **Bitcoin**, **Apple Pay**). 
2. The challenge is to ensure the system is **flexible**, easy to **extend**, and **maintain** as new payment methods are added.

Classes

1. Client Code
2. PaymentStrategy Interface
3. Concrete Payment Strategies

First observe the end goal : Client Code

```jsx
const processor = new Payment(new CardPayment());
processor.process(100);

processor.setStrategy(new UPIPayment());
processor.process(200);
```

## Solution

- **Strategy** is a behavioural design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable
- Strategy Pattern is doing the same thing in different ways.

## Code Solution

```jsx
class Payment{
    paymentStrategy : PaymentStrategy;

    constructor(paymentStrategy: PaymentStrategy){
        this.paymentStrategy = paymentStrategy;
    }

    setStrategy(paymentStrategy: PaymentStrategy): void{
        this.paymentStrategy = paymentStrategy;
    }

    process(amount: number):void{
        this.paymentStrategy.performPayment(amount);
    }
}

interface PaymentStrategy{
    performPayment(amount:number):void;
}

class CardPayment implements PaymentStrategy{
    performPayment(amount:number): void {
        console.log(`Rs.${amount} Payment performed using Card`);
    }
}

class UPIPayment implements PaymentStrategy{
    performPayment(amount:number): void {
        console.log(`Rs.${amount} Payment performed using UPI`);
    }
}

const processor = new Payment(new CardPayment());
processor.process(100);

processor.setStrategy(new UPIPayment());
processor.process(200);
```

## Class Diagram

## Applications

- **Payment Processing System**
    - Strategies: Credit Card Payment, PayPal Payment, Cryptocurrency Payment
- **Shipping System**
    - Strategies: Standard Shipping, Express Shipping, Overnight Shipping
- **Authentication System**
    - Strategies: Email Authentication, Google OAuth, Facebook Login
- **Pricing Engine**
    - Strategies: Regular Pricing, Discount Pricing, Premium Pricing
- **Sorting Algorithms**
    - Strategies: Quick Sort, Merge Sort, Bubble Sort

## Benefits

1. You can swap algorithms used inside an object at runtime.
2. You can isolate the implementation details of an algorithm from the code that uses it.
3. You can replace inheritance with composition.
4. *Open/Closed Principle*. You can introduce new strategies without having to change the context.

## Limitations

1. Not helpful if 2-3 algorithms or strategies are there.
2. In case of limited algorithms, functional programming can be a good choice

## Concept : Tight and Loose Coupling

1. **Composition with Objects of Other Classes**: You can create objects of other classes inside your class and delegate certain responsibilities to them. This helps **reduce tight coupling** because the containing class (the one that holds the object) doesn’t need to know how the contained object works internally, it only interacts with it via its public methods.
2. **Composition with Interfaces**: You can also use interfaces to achieve loose coupling. If your class depends on an interface rather than a concrete class, it becomes **even more flexible** because any object that implements that interface can be passed to it.

```jsx
//1. Composition with objects of other classes
class Engine {
  start(): void {
    console.log("Engine started");
  }
}

class Car {
  private engine: Engine;

  constructor(engine: Engine) {
    this.engine = engine;  // Composing Car with Engine
  }

  drive(): void {
    this.engine.start();  // Delegates behavior to the Engine
    console.log("Car is driving");
  }
}

const engine = new Engine();
const car = new Car(engine);  // Injecting an Engine into the Car
car.drive();

//2. Composition with interface
// In Composition, a class doesn’t inherit from another class, but it contains instances of other classes. This allows the class to use the behavior of other classes without directly being coupled to them. Composition is sometimes described as having a "has-a" relationship, while inheritance is a "is-a" relationship.

interface Vehicle {
  start(): void;
}

class Car implements Vehicle {
  start(): void {
    console.log("Car is starting");
  }
}

class Bike implements Vehicle {
  start(): void {
    console.log("Bike is starting");
  }
}

class Driver {
  private vehicle: Vehicle;

  constructor(vehicle: Vehicle) {
    this.vehicle = vehicle;  // Driver can drive any vehicle that implements Vehicle interface
  }

  drive(): void {
    this.vehicle.start();  // Doesn't care whether it's a Car or a Bike, as long as it follows the Vehicle interface
    console.log("Driving...");
  }
}

const car = new Car();
const bike = new Bike();

const driver1 = new Driver(car);  // Driver drives the car
driver1.drive();

const driver2 = new Driver(bike);  // Driver drives the bike
driver2.drive();

```

## Developer Tips

1. Create /src for .ts files and /dist for .js files
2. Whole code can be in one place, But better to be in different files.

##