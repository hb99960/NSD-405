# NSD S09 LLD : State Design Pattern

**State** is a behavioral design pattern that lets an object alter its behavior when its internal state changes. It appears as if the object changed its class.

## Problem Statement

Earlier, we created objects like `Car` or `DatabaseConnection` **without considering states**—they simply executed behavior without changing dynamically.

Now, with the **State Design Pattern**, we are **interested in its states** and how the object's **behavior changes** based on its current state.

Without State pattern and with state pattern

## Objects (before and After State Pattern)

**Before (Without State Pattern)**

- The `Printer` object had **only one fixed state** (e.g., "Always Out of Paper").
- We didn't **track or modify** its state dynamically.
- It was a **stateless** implementation.

```tsx
class Printer {
    print(document: string): void {
        console.log(`Cannot print "${document}". The printer is out of paper.`);
    }
}
```

- **❌ No state tracking.**
- **❌ No transitions between states.**
- **✅ Simple but limited functionality.**

**Now (With State Pattern)**

- The `Printer` can be in **different states**: `Ready`, `OutOfPaper`, `Printing`.
- The object's **behavior changes dynamically** based on its state.
- The printer **transitions between states** when events occur (e.g., refilling paper).

```tsx
class Printer {
    private state: PrinterState;

    constructor() {
        this.state = new OutOfPaperState(); // Default state
    }

    setState(state: PrinterState): void {
        this.state = state;
    }

    print(document: string): void {
        this.state.print(this, document);
    }

    refillPaper(): void {
        this.state.refillPaper(this);
    }
}
```

- **✅ Object is now "state-aware".**
- **✅ Behavior dynamically changes with state transitions.**
- **✅ New states can be added without modifying existing logic.**

| **Without State Pattern** | **With State Pattern** |
| --- | --- |

| Printer is **always** in one behavior (e.g., "Out of Paper") | Printer **transitions between different states** (e.g., "Ready", "Printing", "Out of Paper"). |
| --- | --- |

| Uses **simple method calls** without considering states. | Behavior **changes dynamically** based on state. |
| --- | --- |

| If logic grows, we use **if-else statements** to check conditions. | No **if-else**, instead behavior is encapsulated in **state classes**. |
| --- | --- |

| Harder to maintain when more conditions are added. | **Easier to extend** with new states (e.g., "Low Ink State", "Paper Jam State"). |
| --- | --- |

## Code Solution

### Without State Design Pattern

```tsx
class User {
    constructor(public role: "admin" | "editor") {}
}

class Document {
    private state: string; // Stores current state

    constructor() {
        this.state = "draft"; // Default state
    }

    publish(currentUser: User): void {
        switch (this.state) {
            case "draft":
                console.log("Document sent for moderation.");
                this.state = "moderation";
                break;

            case "moderation":
                if (currentUser.role === "admin") {
                    console.log("Document published.");
                    this.state = "published";
                } else {
                    console.log("Only admin can publish the document.");
                }
                break;

            case "published":
                console.log("Document is already published.");
                break;

            default:
                console.log("Invalid state.");
        }
    }

    getState(): string {
        return this.state;
    }
}

// Testing the Document Workflow
const editor = new User("editor");
const admin = new User("admin");
const doc = new Document();

console.log(`Current State: ${doc.getState()}`); // "draft"
doc.publish(editor); // Output: "Document sent for moderation."
console.log(`Current State: ${doc.getState()}`); // "moderation"
doc.publish(editor); // Output: "Only admin can publish the document."
console.log(`Current State: ${doc.getState()}`); // "moderation"
doc.publish(admin); // Output: "Document published."
console.log(`Current State: ${doc.getState()}`); // "published"
doc.publish(admin); // Output: "Document is already published."
```

### Without State Design Pattern

```tsx

// State Interface
interface DocumentState {
    publish(document: Document, user: User): void;
}

// Concrete State: Draft
class DraftState implements DocumentState {
    publish(document: Document, user: User): void {
        console.log("Document sent for moderation.");
        document.setState(new ModerationState());
    }
}

// Concrete State: Moderation
class ModerationState implements DocumentState {
    publish(document: Document, user: User): void {
        if (user.role === "admin") {
            console.log("Document published.");
            document.setState(new PublishedState());
        } else {
            console.log("Only admin can publish the document.");
        }
    }
}

// Concrete State: Published
class PublishedState implements DocumentState {
    publish(document: Document, user: User): void {
        console.log("Document is already published.");
    }
}

// User Class
class User {
    constructor(public role: "admin" | "editor") {}
}

// Context Class: Document
class Document {
    private state: DocumentState;

    constructor() {
        this.state = new DraftState(); // Default state
    }

    setState(state: DocumentState): void {
        this.state = state;
    }

    publish(user: User): void {
        this.state.publish(this, user);
    }
}

// Test the Document Workflow
const editor = new User("editor");
const admin = new User("admin");
const doc = new Document();

doc.publish(editor); // Output: "Document sent for moderation."
doc.publish(editor); // Output: "Only admin can publish the document."
doc.publish(admin);  // Output: "Document published."
doc.publish(admin);  // Output: "Document is already published."
```

## Other Example

```tsx
// State Interface
interface State {
    pressButton(fan: Fan): void;
}

// Concrete State: Fan is ON
class OnState implements State {
    pressButton(fan: Fan): void {
        console.log("Turning Fan OFF...");
        fan.setState(new OffState());
    }
}

// Concrete State: Fan is OFF
class OffState implements State {
    pressButton(fan: Fan): void {
        console.log("Turning Fan ON...");
        fan.setState(new OnState());
    }
}

// Context Class
class Fan {
    private state: State;

    constructor() {
        this.state = new OffState(); // Default state
    }

    setState(state: State): void {
        this.state = state;
    }

    pressButton(): void {
        this.state.pressButton(this);
    }
}

// Test the Fan State Transition
const fan = new Fan();
fan.pressButton(); // Output: "Turning Fan ON..."
fan.pressButton(); // Output: "Turning Fan OFF..."
fan.pressButton(); // Output: "Turning Fan ON..."
```

## Class Diagram

## Applications

1. Elevator System Design
2. Vending Machine System Design

## Benefits

- *Single Responsibility Principle*. Organize the code related to particular states into separate classes.
- *Open/Closed Principle*. Introduce new states without changing existing state classes or the context.
- Simplify the code of the context by eliminating bulky state machine conditionals.

## Limitations

- Applying the pattern can be overkill if a state machine has only a few states or rarely changes.