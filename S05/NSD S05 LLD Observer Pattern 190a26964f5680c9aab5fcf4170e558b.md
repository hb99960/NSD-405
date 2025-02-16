# NSD S05 LLD : Observer Pattern

## Problem Statement

In the context of modern smart devices and IoT systems, multiple devices (such as smartphones, tablets, smartwatches, etc.) need to receive and react to updates from a central system, like a weather station. The weather station needs to notify all its observers (smart devices) whenever a weather update occurs. These observers should be able to react to the changes without being tightly coupled to the weather station, allowing for easy addition or removal of new devices.

## Approach

Classes

1. Client Code
2. Subject Interface
3. Concrete Subjects
4. Object Interface
5. Concrete Interface

## Solution

- Publisher + Subscriber = Observer Pattern

Client Code

```jsx
const weatherStation: WeatherStation  = new WeatherStation();

const harshitSmartphone : Smartphone = new Smartphone();
const venuSmartphone : Smartphone = new Smartphone();

const dinkarTablet : Tablet = new Tablet();

weatherStation.attach(harshitSmartphone);
weatherStation.attach(venuSmartphone);
weatherStation.attach(dinkarTablet);

weatherStation.notify();
weatherStation.listAllObservers();
```

## Code Solution

```jsx
interface Subject{
    attach(observer : Observer):void;
    detach(observer : Observer):void;
    notify(observer : Observer):void;
}

class WeatherStation implements Subject{

    private observers: Observer[ ] = [ ];

    attach(observer : Observer): void {
        // .includes() not available in target: es5
        const isExist = this.observers.includes(observer);
        if (isExist) {
            return console.log('Subject: Observer has been attached already.');
        }
        this.observers.push(observer);
        console.log('Subject: Attached an observer.');
    }
    detach(observer : Observer): void {
        const observerIndex = this.observers.indexOf(observer);
        if (observerIndex === -1) {
            return console.log('Subject: Nonexistent observer.');
        }
        this.observers.splice(observerIndex, 1);
        console.log('Subject: Dettached an observer.');
    }
    notify(): void {
        for (const observer of this.observers){
            observer.update(this);
        }
    }

    listAllObservers():void{
        for (const observer of this.observers){
            console.log(observer);
        }
    }

    // toString(): string {
    //     return `WeatherStation with ${this.observers.length} observers`;
    //   }
}

interface Observer{
    update(subject: Subject) : void;
}

class Smartphone implements Observer{
    update(subject: Subject): void {
        console.log(`Smartphone has reacted. Notified by ${subject}`);
    }

}

class Tablet implements Observer{
    update(subject: Subject): void {
        console.log(`Tablet has reacted. Notified by ${subject}`);
    }
}

const weatherStation: WeatherStation  = new WeatherStation();

const harshitSmartphone : Smartphone = new Smartphone();
const venuSmartphone : Smartphone = new Smartphone();

const dinkarTablet : Tablet = new Tablet();

weatherStation.attach(harshitSmartphone);
weatherStation.attach(venuSmartphone);
weatherStation.attach(dinkarTablet);

weatherStation.notify();
weatherStation.listAllObservers();
```

## Class Diagram

## Application

1. **Messaging Systems**
2. **Real-Time Data Monitoring**
3. **Sensor Networks and IoT (Internet of Things)**
4. **Online Collaboration Tools : Google Docs**
5. **Gaming Systems (Player Notifications)**

## Benefit

1. *Open/Closed Principle*. You can introduce new subscriber classes without having to change the publisher’s code (and vice versa if there’s a publisher interface).
2. You can establish relations between objects at runtime.

## Limitation

1. Subscribers are notified in random order.

## SOLID Principles

SOLID is an acronym for five design principles that help in **writing maintainable, scalable, and flexible software**. These principles, introduced by **Robert C. Martin (Uncle Bob)**, are fundamental in **object-oriented design (OOD)**.

## **S - Single Responsibility Principle (SRP)**

**📌 Definition:**

*"A class should have only one reason to change."*

**✅ Why?**

- Makes code **modular** and **easier to maintain**.
- Avoids **god classes** that handle multiple responsibilities.

**🛠 Example (Bad Design - Violates SRP)**

```tsx
class Report {
    generateReport() { /* Logic to generate report */ }
    saveToDatabase() { /* Logic to save report */ }
    sendEmail() { /* Logic to email the report */ }
}

```

**🚀 Fix (Good Design - SRP Applied)**

```tsx
class ReportGenerator {
    generateReport() { /* Logic to generate report */ }
}

class ReportStorage {
    saveToDatabase() { /* Logic to save report */ }
}

class EmailService {
    sendEmail() { /* Logic to email the report */ }
}
```

✅ **Now each class has a single responsibility!**

## **O - Open/Closed Principle (OCP)**

**📌 Definition**

*"Software entities (classes, modules, functions) should be **open for extension but closed for modification**."*

**✅ Why?**

- Encourages **extensibility** without modifying existing code.
- Reduces the risk of introducing **new bugs**.

**🛠 Example (Bad Design - Violates OCP)**

```tsx
class DiscountCalculator {
    calculate(price: number, type: string): number {
        if (type === "gold") return price * 0.9;
        if (type === "silver") return price * 0.95;
        return price;
    }
}

```

🔴 **Problem:** Adding a new discount type requires modifying this class.

**🚀 Fix (Good Design - OCP Applied Using Inheritance/Polymorphism)**

```tsx
typescript
CopyEdit
interface Discount {
    apply(price: number): number;
}

class GoldDiscount implements Discount {
    apply(price: number): number { return price * 0.9; }
}

class SilverDiscount implements Discount {
    apply(price: number): number { return price * 0.95; }
}

class DiscountCalculator {
    calculate(price: number, discount: Discount): number {
        return discount.apply(price);
    }
}

```

✅ **Now, adding a new discount type doesn’t require modifying existing code!**

## Next three i.e. LID in SOLID will be discussed in next sessions