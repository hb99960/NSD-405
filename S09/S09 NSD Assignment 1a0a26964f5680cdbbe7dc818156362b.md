# S09 NSD Assignment

### **What is the primary purpose of the State Design Pattern?**

a) To manage complex mathematical operations

b) To allow an object to change its behavior when its internal state changes

c) To handle multiple inheritance in object-oriented programming

d) To store and retrieve object states from a database

✅ **Answer:** **b) To allow an object to change its behavior when its internal state changes**

### **Which of the following is NOT a characteristic of the State Design Pattern?**

a) It encapsulates different behaviors based on an object’s state

b) It reduces the need for large conditional statements

c) It forces an object to always stay in the same state

d) It allows dynamic transitions between states at runtime

✅ **Answer:** **c) It forces an object to always stay in the same state**

### **Which of the following best describes how the State Pattern differs from a simple switch-case approach?**

a) The State Pattern avoids switch-case statements and instead uses polymorphism

b) The switch-case approach is more scalable than the State Pattern

c) The State Pattern uses a single static state throughout the object's lifecycle

d) The switch-case method is always the recommended way to implement state changes

✅ **Answer:** **a) The State Pattern avoids switch-case statements and instead uses polymorphism**

### **Consider an ATM system implemented using the State Pattern. What would be the best way to handle an "Insufficient Balance" situation when the user attempts a withdrawal?**

a) Add an additional state like `InsufficientBalanceState` and handle the transition

b) Modify the `DispensingCashState` to check the balance and throw an exception

c) Keep the state machine unchanged and let the ATM reject the request without a state transition

d) Remove the state machine and use if-else conditions for balance checking

✅ **Answer:** **a) Add an additional state like `InsufficientBalanceState` and handle the transition**

### **In a media player using the State Pattern, which action would be INVALID for a player in the "Stopped" state?**

a) Play a new track

b) Pause the media

c) Stop the media

d) Exit the player

✅ **Answer:** **b) Pause the media**

### Problem Statement:

You are building a simple vending machine that can be in three states: **Idle, Processing, and Dispensing**.

- The machine starts in the **Idle** state.
- When a user inserts a coin, it moves to the **Processing** state.
- When a selection is made, it moves to the **Dispensing** state.
- After dispensing, it returns to the **Idle** state.

Implement a basic **State Design Pattern** to model this vending machine behavior. Ensure that state transitions are correctly handled through a `VendingMachine` class.

### Submission Guidelines:

- Please submit your Masai Git repository link containing your code.

### Problem Statement:

Design a **Traffic Light System** using the **State Design Pattern**. The traffic light should have three states:

1. **Red** – Vehicles must stop.
2. **Yellow** – Vehicles should slow down.
3. **Green** – Vehicles can move.

The light transitions from **Red → Green → Yellow → Red** in a loop. Implement a `TrafficLight` class that handles these transitions appropriately.

### Submission Guidelines:

- Please submit your Masai Git repository link containing your code.

### Problem Statement:

Design a **Media Player** using the **State Pattern** that supports the following operations:

- **Play State**: If the player is in this state, it should allow playing media.
- **Pause State**: If paused, the player can either resume playing or be stopped.
- **Stop State**: If stopped, media can only be played again from the beginning.

Your implementation should have a `MediaPlayer` class with state transitions based on user actions (`play()`, `pause()`, `stop()`).

### Submission Guidelines:

- Please submit your Masai Git repository link containing your code.

### Problem Statement:

A bank's ATM is implemented using the **State Pattern**, but it's not functioning correctly. The ATM should have the following states:

1. **Idle** – Waiting for a user.
2. **Card Inserted** – Waiting for PIN entry.
3. **Authenticated** – Allowing cash withdrawal.
4. **Dispensing Cash** – Dispensing money.
5. **Idle (again)** – Returning to the starting state after a transaction.

However, in the given implementation:

- The ATM transitions directly from **Idle → Authenticated**, skipping the PIN verification step.
- It does not return to the **Idle** state after dispensing cash.

Your task is to **identify and fix** these issues in the provided code.

### Submission Guidelines:

- Please submit your Masai Git repository link containing the corrected code and a brief explanation of what you fixed.

### Problem Statement:

Design a **Smart Home Automation System** using the **State Design Pattern**. The system should manage a **Smart Light**that can be in different states based on user actions and external conditions:

1. **Off State** – The light is turned off.
2. **On State** – The light is turned on manually.
3. **Motion Detection State** – The light turns on automatically when motion is detected.
4. **Brightness Adjustment State** – If it’s daytime, brightness is reduced; at night, it’s increased.

Requirements:

- Implement state transitions for the light based on user input and environmental conditions.
- The system should be scalable, allowing future state additions like "Energy Saver Mode."

### Submission Guidelines:

- Please submit your Masai Git repository link containing your implementation and a short explanation of your design choices.