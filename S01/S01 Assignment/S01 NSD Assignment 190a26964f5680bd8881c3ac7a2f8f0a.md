# S01 NSD Assignment

1. TypeScript is ________ to JavaScript using the TypeScript compiler (`tsc`).
    1. Interpreted
    2. Transpiled
    3. Compiled
    4. Converted
    
    Answer : b. **Transpiled**
    
2. JavaScript is ________ and ________ by engines like V8 (in Chrome and Node.js) or other browser engines.
    1. Transpiled, Executed
    2. Interpreted, Executed
    3. Compiled, Executed
    4. Parsed, Transpiled
    
    Answer : **2. Interpreted, Executed**
    
3. Which of the following requires compilation step?
    1. Javascript
    2. Typescript
    3. Both
    4. None of them
    
    Answer : **2. TypeScript**
    
4. **Which of the following is a compile-time error?**
    
    A) Division by zero
    
    B) Attempting to access a property of `null`
    
    C) Trying to assign a string to a number variable in TypeScript
    
    D) Using an undefined function
    
    Answer : **C) Trying to assign a string to a number variable in TypeScript**
    
5. **Which of the following is a run-time error?**
    
    A) Missing semicolon in JavaScript
    
    B) Using an undeclared variable in TypeScript
    
    C) Trying to access a property on `null`
    
    D) Typing mismatch in TypeScript
    
    Answer : **C) Trying to access a property on `null`**
    
6. Explain the process of running a TS file. (Write steps)
7. Convert this code to avoid the errors at run-time

```jsx
// Question : 
function add(a, b) {
  return a + b;
}

let result = add(10, "5"); 
console.log(result);  
```

```jsx
// Solution : 
function add(a: number, b: number): number {
  return a + b;
}

let result = add(10, 5);  // Correct: Both arguments are numbers
console.log(result);  // Output: 15

let invalidResult = add(10, "5");  // Error: Argument of type 'string' is not assignable to parameter of type 'number'

```

1. What will be the output?

```jsx
let unknownValue: unknown = "Hello, TypeScript!";
let anyValue: any = "Hello, TypeScript!";

// 1. What will happen if we call `.toUpperCase()` on both values?
console.log(unknownValue.toUpperCase());  // What will happen here?
console.log(anyValue.toUpperCase());     // What will happen here?

// 2. 
let myValue: unknown = 100;
console.log(myValue + 1);  // What will happen here?

```

1. Write a TypeScript function that takes a number as input and returns whether the number is **even** or **odd**.
2. Write a TypeScript function that takes an array of numbers and returns the **sum** of all even numbers in the array. The function should return `0` if no even numbers are found.
3. Write a TypeScript class `Person` with the following features:
    1. The class should have `name` (string) and `age` (number) as properties.
    2. It should have a method `greet()` that returns a greeting message including the person's name and age.
    3. Additionally, implement a static method `isAdult()` that checks if the person is an adult (age ≥ 18).