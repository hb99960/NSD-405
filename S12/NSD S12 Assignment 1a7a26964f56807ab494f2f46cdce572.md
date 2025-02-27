# NSD S12 Assignment

Which of the following best describes the **State Design Pattern**?

### **Options:**

1. It allows an object to change its behavior when its internal state changes.
2. It provides an interface for creating families of related objects.
3. It is used to create multiple instances of a class using cloning.
4. It ensures that a class has only one instance and provides a global point of access to it.

### **Solution:**

✅ **Option 1:** The **State Pattern** allows an object to change its behavior dynamically based on its internal state.

---

## **Problem Statement 2:**

Which design pattern is most suitable for creating **different types of objects without specifying their exact classes**?

### **Options:**

1. State Pattern
2. Factory Pattern
3. Prototype Pattern
4. Strategy Pattern

### **Solution:**

✅ **Option 2:** The **Factory Pattern** is used to create objects without specifying their exact concrete classes.

---

## **Problem Statement 3:**

What is the main advantage of using the **Abstract Factory Pattern** over the **Factory Method Pattern**?

### **Options:**

1. It provides a single method to create all types of objects.
2. It allows the creation of related objects without specifying their concrete classes.
3. It eliminates the need for subclasses.
4. It does not require object creation at runtime.

### **Solution:**

✅ **Option 2:** The **Abstract Factory Pattern** helps create families of related objects without specifying their concrete classes.

---

## **Problem Statement 4:**

Which of the following is **NOT** a characteristic of the **Prototype Pattern**?

### **Options:**

1. It is used to create new objects by copying an existing object.
2. It avoids the overhead of creating objects from scratch.
3. It relies on interfaces for object creation.
4. It requires a large number of subclasses for different object types.

### **Solution:**

✅ **Option 4:** The **Prototype Pattern** reduces the need for multiple subclasses by using object cloning, not increasing them.

---

## **Problem Statement 5:**

Which of the following is an example of the **State Design Pattern**?

### **Options:**

1. A traffic light system changing between red, yellow, and green.
2. A factory that creates different types of vehicles based on input.
3. A GUI theme switcher that creates new themes dynamically.
4. A database connection manager that ensures only one connection instance exists.

### **Solution:**

✅ **Option 1:** The **State Pattern** is used in **traffic light systems**, where the behavior changes based on the current state (red, yellow, green).

---

## **Problem Statement 6:**

Which design pattern **helps in reducing object creation costs by reusing existing objects**?

### **Options:**

1. Abstract Factory Pattern
2. Prototype Pattern
3. State Pattern
4. Factory Pattern

### **Solution:**

✅ **Option 2:** The **Prototype Pattern** reduces object creation costs by cloning existing objects instead of creating new ones.

---

## **Problem Statement 7:**

In the **Factory Pattern**, how is object creation delegated?

### **Options:**

1. Through a **centralized factory class** that determines the object to be created.
2. By switching between different state objects at runtime.
3. By creating an instance from an existing object through cloning.
4. By using subclasses to create different object families.

### **Solution:**

✅ **Option 1:** The **Factory Pattern** uses a centralized **factory class** to create objects without exposing their concrete implementations.

---

## **Problem Statement 8:**

Which of the following correctly represents the **Abstract Factory Pattern**?

### **Options:**

1. It provides a factory for creating objects belonging to the same family.
2. It allows an object to change its behavior based on state changes.
3. It provides a way to clone existing objects.
4. It ensures that only one instance of a class exists.

### **Solution:**

✅ **Option 1:** The **Abstract Factory Pattern** provides a factory for creating objects belonging to the same family (e.g., UI themes for different operating systems).

## Problem Statement

Snake and Ladder is a classic board game played on a **N x N board**. Players take turns rolling a dice and move forward based on the number they roll. The objective is to reach the last cell on the board. However, the game includes **snakes** and **ladders**:

- **Ladders:** If a player lands on a cell with the bottom of a ladder, they move up to the top of the ladder.
- **Snakes:** If a player lands on a cell with a snake’s head, they move down to the snake’s tail.
- The game continues until one player reaches the final cell.

### Submission guidelines

- Submit your Masai Git directory link.