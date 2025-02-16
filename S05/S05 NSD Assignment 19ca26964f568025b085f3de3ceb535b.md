# S05 NSD Assignment

### **1. Problem Statement:**

What is the primary purpose of the Observer Pattern?

**Options:**

1. To ensure only one instance of a class exists
2. To define a one-to-many dependency between objects
3. To encapsulate object creation logic
4. To provide a way to access elements sequentially

### **Solution: 2.  To define a one-to-many dependency between objects**

> The Observer Pattern ensures that when the state of one object (subject) changes, all its dependent objects (observers) are notified and updated automatically.
> 

### **2. Problem Statement:**

Which of the following correctly describes the relationship between the Subject and the Observer in the Observer Pattern?

**Options:**

1. The Observer is dependent on the Subject
2. The Subject is dependent on the Observer
3. Both Subject and Observer are independent
4. The Subject and Observer communicate through a third-party class

### **Solution:**

**1. The Observer is dependent on the Subject**

> The Observer relies on the Subject for updates whenever the state of the Subject changes.
> 

### **3. Problem Statement:**

How can we avoid memory leaks when implementing the Observer Pattern in TypeScript?

**Options:**

1. By not adding too many observers
2. By keeping references to all observers in a global list
3. By properly unsubscribing observers when they are no longer needed
4. By forcing garbage collection manually

**Solution:**

**3. By properly unsubscribing observers when they are no longer needed**

Failing to unsubscribe observers can lead to memory leaks because they may still hold references to subjects.

### 4. Which of the following best describes how the Observer Pattern improves software design?

**Options:**

1. It reduces tight coupling between objects
2. It forces all observers to always receive updates
3. It ensures that subjects can only have one observer
4. It eliminates the need for an interface in object communication

**Solution:**

**1. It reduces tight coupling between objects**

The Observer Pattern promotes loose coupling by allowing observers to dynamically subscribe or unsubscribe from a subject without modifying it.

### **5. Problem Statement:**

What is a major drawback of the Observer Pattern?

**Options:**

1. It tightly couples observers and subjects
2. It requires observers to implement multiple interfaces
3. It can lead to performance issues if there are too many observers
4. It does not allow dynamic subscriptions

**Solution:**

**3. It can lead to performance issues if there are too many observers**

Notifying a large number of observers can slow down performance if the update operation is costly.

### **1. Easy Coding Question**

### **Problem Statement:**

Implement a simple **Observer Pattern** in TypeScript where a `NewsAgency` (Subject) notifies multiple `Subscribers`(Observers) when news is published.

### **2. Easy Complete the Code Question**

### **Problem Statement:**

Complete the `removeObserver` method in the following Observer Pattern implementation to properly unsubscribe an observer.

### **Incomplete Code:**

```tsx
interface Observer {
    update(message: string): void;
}

class Subject {
    private observers: Observer[] = [];

    addObserver(observer: Observer) {
        this.observers.push(observer);
    }

    removeObserver(observer: Observer) {
        // Complete this method
    }

    notifyObservers(message: string) {
        this.observers.forEach(observer => observer.update(message));
    }
}
```

### **3. Medium Problem Statement:**

Modify the Observer Pattern implementation so that an observer can pause and resume receiving updates from a subject without unsubscribing completely.

```tsx
interface Observer {
    update(data: string): void;
    pause(): void; // Implement this
    resume(): void; // Implement this
}

class Subject {
    private observers: Map<Observer, boolean> = new Map(); // Boolean indicates active/inactive state

    addObserver(observer: Observer) {
        this.observers.set(observer, true); // By default, observers are active
    }

    removeObserver(observer: Observer) {
        this.observers.delete(observer);
    }

    notifyObservers(data: string) {
        this.observers.forEach((isActive, observer) => {
            if (isActive) {
                observer.update(data);
            }
        });
    }

    pauseObserver(observer: Observer) {
        // Implement this method to mark observer as inactive
    }

    resumeObserver(observer: Observer) {
        // Implement this method to mark observer as active again
    }
}

class ConcreteObserver implements Observer {
    constructor(private name: string) {}

    update(data: string): void {
        console.log(`${this.name} received update: ${data}`);
    }

    pause(): void {
        // Implement this method to stop receiving updates
    }

    resume(): void {
        // Implement this method to start receiving updates again
    }
}

// Usage Example
const subject = new Subject();
const observer1 = new ConcreteObserver("Observer 1");
const observer2 = new ConcreteObserver("Observer 2");

subject.addObserver(observer1);
subject.addObserver(observer2);

subject.notifyObservers("Initial Update"); // Both should receive

subject.pauseObserver(observer1); // Observer 1 should stop receiving updates

subject.notifyObservers("Second Update"); // Only Observer 2 should receive

subject.resumeObserver(observer1); // Observer 1 should start receiving updates again

subject.notifyObservers("Final Update"); // Both should receive

```

### 4. Medium

### **Problem Statement:**

Modify the Observer Pattern so that an observer can specify the type of events it wants to subscribe to. Implement an event-based Observer Pattern where observers only receive updates for the events they subscribed to. 

### 5. Hard

### **Problem Statement:**

Implement an **Observable Class** in TypeScript that allows multiple types of observers (for different event categories) and supports **unsubscribeAll()** to remove all observers of a particular type.