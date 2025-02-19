# NSD S08 Assignment

### **Q1: What is the primary purpose of using `#` in TypeScript?**

A) To indicate a protected property

B) To define a truly private field that cannot be accessed outside the class

C) To create a static property in a class

D) To specify a global variable

✅ **Answer:** **B) To define a truly private field that cannot be accessed outside the class**

📌 **Explanation:**

- The `#` symbol is used to declare **private fields** in TypeScript, ensuring that they **cannot be accessed or modified outside the class**, even at runtime.

---

### **Q2: Which of the following statements about the `static` keyword in TypeScript is true?**

A) `static` properties belong to an instance of a class

B) `static` methods can only be accessed inside the class

C) `static` members are shared across all instances of a class

D) `static` properties are the same as private properties

✅ **Answer:** **C) `static` members are shared across all instances of a class**

📌 **Explanation:**

- `static` members **belong to the class itself**, not an instance.
- They **can be accessed using the class name** (`ClassName.property`).
- All instances **share the same static members**.

---

### **Q3: Which of the following best describes the Strategy Design Pattern?**

A) It ensures that only one instance of a class exists

B) It provides an interface for creating families of related objects

C) It allows selecting different algorithms at runtime

D) It restricts object creation to subclasses only

✅ **Answer:** **C) It allows selecting different algorithms at runtime**

📌 **Explanation:**

- The **Strategy Pattern** defines a **family of algorithms** and allows them to be **interchanged dynamically at runtime**.
- **Example:** A parking system where different strategies (`RandomParkingStrategy`, `FirstAvailableStrategy`) decide where a vehicle should be parked.

---

### **Q4: What is the main goal of the Factory Design Pattern?**

A) To enforce a single instance of a class

B) To provide an interface for creating related objects without specifying their exact class

C) To allow dynamic method dispatch

D) To create multiple instances of a class directly using `new`

✅ **Answer:** **B) To provide an interface for creating related objects without specifying their exact class**

📌 **Explanation:**

- The **Factory Pattern** **centralizes object creation**, ensuring that the client code **does not need to know the concrete class being instantiated**.
- It **hides the creation logic** and **returns an instance based on input parameters**.

---

### **Q5: How does the Singleton Design Pattern ensure that only one instance of a class exists?**

A) By using the `static` keyword on all class properties

B) By making the constructor private and providing a static `getInstance` method

C) By using multiple constructors with different access levels

D) By restricting object creation to factory classes only

✅ **Answer:** **B) By making the constructor private and providing a static `getInstance` method**

📌 **Explanation:**

- The **Singleton Pattern** prevents **multiple instances** by making the **constructor private** and allowing instance creation **only through a static method (`getInstance()`)**.
- This ensures that **only one object exists globally** throughout the application.

---

### **🔹 Problem: Implement a User Authentication Class**

Create a `User` class where

- `#password` is **private**, so it **cannot be accessed directly**.
- The constructor initializes `username` and `#password`.
- Provide a `checkPassword(input: string): boolean` method that **validates the password**.
- If the password matches, return `true`, otherwise return `false`.

### **🔹 Example Usage**

```tsx
const user = new User("Alice", "secure123");
console.log(user.checkPassword("secure123")); // ✅ Output: true
console.log(user.checkPassword("wrongpass")); // ✅ Output: false
console.log(user.#password); // ❌ Error: Private field '#password' must be declared in an enclosing class
```

### **📌 Submission Guidelines**

- Ensure `#password` is truly private.
- Implement the `checkPassword()` method properly.

---

### **🔹 Problem: Implement a Counter Class**

Create a `Counter` class where:

- `static count` tracks the total number of objects created.
- The constructor **increments `count`** each time a new object is instantiated.
- Provide a `static getCount(): number` method to return the total count.

### **🔹 Example Usage**

```tsx
const obj1 = new Counter();
const obj2 = new Counter();
console.log(Counter.getCount()); // ✅ Output: 2

const obj3 = new Counter();
console.log(Counter.getCount()); // ✅ Output: 3
```

### **📌 Submission Guidelines**

- Implement `static count` correctly.
- Ensure `getCount()` returns the correct count.

---

---

### **🔹 Problem: Implement a Shape Factory**

Create a **Factory Pattern** to generate different shapes:

- Define a `Shape` interface with a `draw()` method.
- Implement `Circle`, `Rectangle`, and `Square` classes.
- A `ShapeFactory` class should create **the correct shape object** based on input.

### **🔹 Example Usage**

```tsx
const circle = ShapeFactory.createShape("circle");
circle.draw();  // ✅ Output: "Drawing a Circle"

const rectangle = ShapeFactory.createShape("rectangle");
rectangle.draw();  // ✅ Output: "Drawing a Rectangle"
```

### **📌 Submission Guidelines**

- Implement the **Factory Pattern properly**.
- Ensure **`ShapeFactory.createShape()` returns the correct instance**.

---

### **🔹 Problem: Implement a Logger Singleton**

Create a `Logger` class that:

- **Ensures only one instance** is created.
- Logs messages with timestamps.
- Provides a `getLogs()` method to retrieve logs.

### **🔹 Example Usage**

```tsx
const logger1 = Logger.getInstance();
logger1.log("App started");

const logger2 = Logger.getInstance();
logger2.log("User logged in");

console.log(logger1.getLogs());
// ✅ Output:
// [2024-02-19 10:00:00] App started
// [2024-02-19 10:01:00] User logged in

console.log(logger1 === logger2);  // ✅ Output: true (Same instance)
```

### **📌 Submission Guidelines**

- Implement the **Singleton Pattern correctly**.
- Ensure `getLogs()` returns all logs in order.

---

### **🔹 Problem: Implement a Sorting Strategy System**

You need to implement a system that supports **multiple sorting strategies** for a list of numbers.

- Define a `SortingStrategy` interface with a method `sort(arr: number[]): number[]`.
- Implement at least **three sorting strategies**:
    1. **Bubble Sort**
    2. **Quick Sort**
    3. **Built-in Sort (JavaScript `.sort()`)**
- A `Sorter` class should allow **switching strategies dynamically**.

### **🔹 Example Usage**

```tsx
const sorter = new Sorter(new BubbleSort());
console.log(sorter.sort([5, 3, 8, 1]));  // ✅ Output: [1, 3, 5, 8] (Bubble Sort)

sorter.setStrategy(new QuickSort());
console.log(sorter.sort([5, 3, 8, 1]));  // ✅ Output: [1, 3, 5, 8] (Quick Sort)
```

### **📌 Submission Guidelines**

- Implement at least **three sorting algorithms**.
- Ensure **`Sorter` can switch strategies dynamically**.

---

### **🔹 Problem: Implement a Notification Factory**

You need to create a **Notification Factory** that can generate different types of notifications.

- Define a `Notification` interface with a method `send(message: string)`.
- Implement three notification types:
    1. **Email Notification**
    2. **SMS Notification**
    3. **Push Notification**
- A `NotificationFactory` should **return the correct notification object** based on input.

### **🔹 Example Usage**

```tsx
const emailNotif = NotificationFactory.createNotification("email");
emailNotif.send("Welcome!");  // ✅ Output: "Sending Email: Welcome!"

const smsNotif = NotificationFactory.createNotification("sms");
smsNotif.send("Your OTP is 1234");  // ✅ Output: "Sending SMS: Your OTP is 1234"
```

### **📌 Submission Guidelines**

- Implement at least **three notification types**.
- Ensure `NotificationFactory.createNotification()` **returns the correct instance**.

---

###