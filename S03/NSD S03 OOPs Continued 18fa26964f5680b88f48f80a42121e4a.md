# NSD S03 OOPs Continued

## Recall Inheritance and Interface

- Refer S02 notes

## Design Principle : Encapsulate what varies

The design principle **"Encapsulate what varies"** suggests that you should isolate the parts of a system that are likely to change or vary over time, and manage them separately from the parts that remain stable. In the context of the **Duck Simulation Game**, this principle can be applied to the different behaviors of ducks, such as **flying** and **quacking**. Since different types of ducks may have different flying abilities or sounds (e.g., a rubber duck can't fly, a wooden duck might not quack), instead of hardcoding these behaviors into each duck class, we can **encapsulate** them into separate classes.

## Problem

You started developing a **Duck Simulation App** to model various duck types, such as the **Indian Duck** and the **American Duck**, each with specific behaviors (like different sounds and flight abilities). The initial design worked well as long as you only had these two types. However, new demands kept coming in, and the requirements soon became more complex.

As the app grew, new duck types were requested by users, each with its own specific behaviors. For example:

- **City Duck**, which behaves differently in urban environments (perhaps no flying and different sounds).
- **Wooden Duck**, which can’t fly but makes a squeaky sound.
- **Rubber Duck**, which doesn't fly, doesn't make a sound, and only floats.
- **Mallard Duck**, which behaves differently, especially in how it moves and interacts with the environment.

Soon, the `Duck` class became too bloated. To accommodate these new ducks, the class grew larger with lots of **if** and **else**conditions for each duck type. The `Duck` class now contained a lot of logic to manage all possible combinations of behaviors, such as:

- Flying behaviors
- Quacking sounds (mute, squeaky, loud)
- Swimming/floating behaviors

This not only made the code hard to maintain but also made adding **new duck types** or modifying **existing behaviors**difficult without risking breaking something else. The growing complexity led to:

- **Code duplication**: The same behaviors were defined for each duck type, leading to repetition.
- **Rigid design**: Each new duck type required modifying the core `Duck` class, violating the **Open/Closed Principle**(the app was not open for extension but was closed for modification).
- **Merge conflicts**: As the development team grew, each addition to the duck types or behaviors caused frequent conflicts when working on the shared `Duck` class.

We have 100 types of duck. All will have same swim behaviour. Let say 80 ducks have same fly behaviour(FlyWithWings), 20 have same fly behaviour (FlyNoWay). Will you write the definition of all those classes? No
 * What is the solution?
 * FlyWithWings and FlyNoWay has concrete implementation and we need to reuse this code. So, we will have these 2 classes under the interface FlyBehaviour

```jsx
interface Flyable {
    fly(): string;
}

export class FlyNoWay implements Flyable{
    fly(): string {
        return "Sorry I don't fly!!";
    }

}

export class FlyWithWings implements Flyable{
    fly(): string {
        return "I fly with wings";
    }

}
```

## Public vs Private keyword

- **`public`**:
    - The `public` keyword makes a class member (property or method) **accessible from anywhere**—both inside and outside the class.
    - By default, all class members in TypeScript are `public` if no access modifier is specified.
- **`private`**:
    - The `private` keyword restricts access to the class member. It can only be accessed **within the class itself** and not from outside.
    - Useful for **encapsulating** internal state or behavior that should not be modified directly.

```jsx
export class Duck{

    public flyable:Flyable;
    public quackable: Quackable;

    constructor(flyable: Flyable, quackable: Quackable) {
        this.flyable = flyable;
        this.quackable = quackable;
    }
}

// public: Accessible everywhere.
// private: Accessible only within the class.
```

## Setting Behaviour Dynamically

It means changing the behaviour of the object on the fly. In other words, while the application is running, changing the behaviour of object. For example,

```jsx
const myDuck = new Duck(new FlyNoWay(), new MuteQuack());
console.log(`myDuck ${myDuck.makeSound()}`);
console.log(`myDuck ${myDuck.performFly()}`);

// changing the behaviour
myDuck.setFlyBehaviour(new FlyWithWings());
console.log(`myDuck new : ${myDuck.performFly()}`);
```

## Abstract class

### **Abstract Class**:

- An **abstract class** is a class that cannot be instantiated directly. It provides a **base** for other classes to inherit from.
- It can have both **abstract methods** (methods without implementation) and **concrete methods** (methods with implementation).
- **Use Case**: When you need to reuse code from a parent class while enforcing a contract for subclasses to implement specific methods.

### **Concrete Class**:

- A **concrete class** is a class that can be instantiated and provides full implementations for all methods.
- **Use Case**: When you need to create objects with fully implemented methods.

**Why Use an Abstract Class?**

- **Abstract class** gives you the **best of both worlds**: you can reuse code (like inheritance) and enforce method implementations (like interfaces), making it **more flexible**.

## Polymorphism

- Overloading
    
    **Method Overloading** allows a class to have **multiple methods** with the same name but with different **parameters**. This helps achieve flexibility, where the method can behave differently based on the input type or number of arguments.
    
    - **In TypeScript**, method overloading is not supported directly, but we can achieve it using **optional parameters** or **overloading signatures**.
    - Overloading is about having the **same method name** but different **parameter types** or **number of parameters**.
- Overriding
    
    **Method Overriding** occurs when a subclass provides a **specific implementation** for a method that is already defined in its **parent class**. This allows subclasses to change or **extend the behavior** of inherited methods.
    
    - **Overriding** is a key feature of **polymorphism** because it allows objects of different classes to behave differently while sharing the same method signature.
- Object Substitution (Polymorphic behaviour)
    
    **Object Substitution** refers to the ability to **substitute** an object of a **subclass** wherever an object of the **parent class** is expected. This is a fundamental principle of **polymorphism**, as it allows a class hierarchy to behave in a flexible manner. A **parent class reference** can hold an object of any **child class** that inherits from it, enabling dynamic method resolution.
    
    ```jsx
    // Object substitution (polymorphism)
    let duck: Duck = new MallardDuck("Mallard");
    console.log(duck.makeSound());  // Output: Mallard is quacking loudly!
    ```
    
- Other ways to have polymorphism using Supertypes
    - Aim is program to supertype
    - Supertype can be Regular Class, Abstract class, Interface
    
    ```jsx
    // Example of supertypes :  
    // Method - 1 : Abstract class
    abstract class Animal {
        abstract void makeSound();  // Abstract method (no body)
    }
    
    // Method - 2 : Interface
    interface Animal {
        void makeSound();
    }
    
    // Method - 1 : Regular class
    class Animal {
        void makeSound() {
            System.out.println("Some generic animal sound");
        }
    }
    
    Animal animal = new Dog();
    ```
    

| Interface | Abstract Class |
| --- | --- |
| All methods are abstract | Some methods can be abstract and others can be concrete |
|  | Can not create objects from abstract class |

## Project : Duck Simulation Game

```jsx
// import { Flyable } from "./flyable";

export class Duck{

    public flyable:Flyable;
    public quackable: Quackable;

    constructor(flyable: Flyable, quackable: Quackable) {
        this.flyable = flyable;
        this.quackable = quackable;
    }

    swim(): string{
        return `swim!! swim!!`;
    }

    performFly(): string {
        return this.flyable.fly();
    }

    makeSound(): string{
        return this.quackable.quack();
    }

    setFlyBehaviour(flyable:Flyable):void{
        this.flyable = flyable;
    }

    setQuackBehaviour(quackable:Quackable):void{
        this.quackable = quackable;
    }

}

// First Interface
interface Flyable {
    fly(): string;
}

export class FlyNoWay implements Flyable{
    fly(): string {
        return "Sorry I don't fly!!";
    }

}

export class FlyWithWings implements Flyable{
    fly(): string {
        return "I fly with wings";
    }
}

// Second Interface
interface Quackable {
    quack(): string;
}

export class IndianDuck extends Duck{
    constructor(){
        super(new FlyWithWings(), new Quack()); 
    }
    makeSound(): string {
        return "Sound of Indian Duck!";
    }
}

export class AmericanDuck extends Duck implements Flyable{
    fly(): string {
        return "Normal Fly";
    }

}

export class RubberDuck extends Duck {
   
    constructor(){
        super(new FlyNoWay(), new Quack());
    }

    makeSound(): string {
        return "Beep!! Beep!!";
    }
    
}

export class WoodDuck extends Duck{
    constructor(){
        super(new FlyNoWay(), new MuteQuack());
    }
}

// Now you can create any type of duck and that will reusable and maintainable code

//compile-time
const indianDuck = new IndianDuck();
console.log(indianDuck.makeSound());
console.log(`${indianDuck.performFly()}`);

// run-time
const americanDuck = new Duck(new FlyNoWay(), new MuteQuack());
console.log(`Sound made by American Duck = ${americanDuck.makeSound()}`);
console.log(`${americanDuck.performFly()}`);

const rubberDuck = new Duck(new FlyNoWay, new Quack());
console.log(`${rubberDuck.makeSound()}`);
console.log(`${rubberDuck.performFly()}`);

const myDuck = new Duck(new FlyNoWay(), new MuteQuack());
console.log(`myDuck ${myDuck.makeSound()}`);
console.log(`myDuck ${myDuck.performFly()}`);
myDuck.setFlyBehaviour(new FlyWithWings());
console.log(`myDuck new : ${myDuck.performFly()}`);

```