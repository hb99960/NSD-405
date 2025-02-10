# NSD S01 TypeScript + OOPs

### Why System Design?

- Scalable Systems
- Maintainable code
- Low level Design : Code level
- High Level Design : Hardware level

### Find difference between two codes

- Start with the keywords. How many extra keywords you can find?
- Which code is lengthy? Left one or Right one?
- Are both looking like Javascript, huh?

![Screenshot 2025-02-01 at 6.32.47 PM.png](NSD%20S01%20TypeScript%20+%20OOPs%20627ab335276b470b8f4afaf4d77d71e9/Screenshot_2025-02-01_at_6.32.47_PM.png)

### Why do we need TS?

Is JS is a bad language? Do I have to learn a new programming language now?

- No, JS is still widely used language. You don’t need to abandon JS, **learning TypeScript** or other new technologies is **complementary** to JavaScript and enhances your skills, especially for larger projects, but it doesn't replace JavaScript. Let’s understand the exact need of TS with simple but useful example.
- You can assign number and string to a same variable which is problematic in larger code base.
- In TS, this is detected at the compiling stage itself.

![Screenshot 2025-02-01 at 1.04.21 PM.png](NSD%20S01%20TypeScript%20+%20OOPs%20627ab335276b470b8f4afaf4d77d71e9/Screenshot_2025-02-01_at_1.04.21_PM.png)

## Introduction to Typescript

### What is Typescript?

1. TypeScript is a statically-typed superset of JavaScript that enhances the development experience by catching errors at compile time. It helps in ensuring robust and maintainable code. (Sometimes, definitions are important especially in Interviews)
    - statically-typed = you must specify the type of variables, function parameters, and return values, or TypeScript will assume the types automatically.
    - superset =
        
        ![Screenshot 2025-02-01 at 6.46.41 PM.png](NSD%20S01%20TypeScript%20+%20OOPs%20627ab335276b470b8f4afaf4d77d71e9/Screenshot_2025-02-01_at_6.46.41_PM.png)
        
    - Recall!! compile time = converting high level language to machine language (0s and 1s)
        - High-level languages are JS, TS, Java, Python, etc
    
    ```jsx
    // Example of Statically typing in TS
    let name: string = "Alice";  // Type 'string' is explicitly declared
    let age = 25;  // TypeScript infers 'age' as 'number'
    ```
    
    - Goal is to reduce the errors. Recall the major kinds of errors. Compile time (Syntax) and Run-time (logical error). We will identify these errors at compile time only. This thing is called Static type checking.
    - Try to think why errors at run-time are problematic than compile time
2. File extension of typescript is .ts (shorthand of typescript)

### Benefits of Typescript

1. Error detection because of strong type checking
2. Support of Object Oriented Programming concepts (OOPs)
    1. OOPs what it will do is it convert the real life objects into programming objects through which we will softwares eventually to solve real-life problems.
    2. For example, Car in real life will be converted to Object in our code as Car.ts file (Hold your thoughts for one more session)

Therefore, In a simple manner Typescript = Type checking + OOPs. However it is not limited to only these two things.

### What Typescript is not?

1. **TypeScript is not a fully-compiled language**: TypeScript needs to be **transpiled** (converted) into JavaScript before it can run in browsers or on Node.js. It doesn't directly execute code.
    - Recall!! Compile time = converting high level language to machine language (0s and 1s) (done by V8 engine in RAM)
    - Recall!! Runtime = running of compiled code in the CPU
    - Transpiled = converting one high level language to another high level language. In our case, .ts file to .js file is transpilation
    
    ![Screenshot 2025-02-01 at 6.34.27 PM.png](NSD%20S01%20TypeScript%20+%20OOPs%20627ab335276b470b8f4afaf4d77d71e9/Screenshot_2025-02-01_at_6.34.27_PM.png)
    
2. TypeScript is not a runtime type checker
    1. meaning after transpilation and compilation, type checking will not be done. 

## Getting Started with Typescript

## Installation

1. Verify Node.js installation
    
    ```jsx
    node -v
    npm -v
    ```
    
2. Open your terminal/command prompt and run:

```jsx
npm install -g typescript
```

- -g : we are telling npm to install typescript globally. But, Why to install it globally?
- so that the `tsc` (TypeScript compiler) command is available system-wide, allowing you to use TypeScript in any project or directory without needing to install it separately each time.
1. Verify **TypeScript Installation**:

```jsx
tsc -v
```

## **Initialise a TypeScript Project**

1. Open **VS Code**.
2. Create a new folder for your TypeScript project:
    - You can do this directly in **VS Code** by going to `File` -> `Open Folder...` and selecting a location, or open a terminal inside **VS Code** and run:
    
    ```jsx
    mkdir my-ts-project
    cd my-ts-project
    ```
    
3. Run the following command to initialize the TypeScript project:

```jsx
tsc --init
```

1. Edit tsconfig.json

```jsx
{
  "compilerOptions": {
    "module": "CommonJS",  
    "target": "es6",
    "esModuleInterop": true,
    "moduleResolution": "node",
    "allowJs": true,
    "outDir": "./dist",
    "noEmitOnError": true,         
      "removeComments": true,        
      "strict": true,                      
      "skipLibCheck": true 
  },
  "include":[
      "*.ts",
      "src/*.ts"
  ],
  "exclude" : [
      "nodule_modules"
  ]
}
```

### How to run a TS file?

Create a new file with a `.ts` extension in the project folder:

1. Inside **VS Code**, click `File` -> `New File`.
2. Save the file as `app.ts` or any other name with `.ts` extension.
3. Write some TypeScript code, for example:

```jsx
let greeting: string = "Hello, TypeScript!";
console.log(greeting);
```

1. Run the following command, To compile the TypeScript code into JavaScript :

```jsx
tsc
```

1. **Run the Compiled JavaScript**

```jsx
node app.js
```

### TypeScript : Language

So, How many types are there in typescript? Actually a lot!!! But, don’t worry if you know the important ones you are set for the interviews and developing application. However, One should be familiar with various types just the basic idea in one liner not in the code application. Here is the list : 

### 1. **Primitive Types**:

- `string`: Represents text data.
- `number`: Represents numerical values.
- `boolean`: Represents `true` or `false`.
- `null`: Represents a null value.
- `undefined`: Represents an undefined value.
- `symbol`: A unique and immutable primitive value (used for unique identifiers).
- `bigint`: Represents large integers.

```jsx
let name: string = "Alice";
let age: number = 25;
let isActive: boolean = true;
```

### 2. **Object Types**:

- `object`: Represents any non-primitive type (arrays, functions, etc.).
- `Array`: Represents an array of elements.
- `tuple`: Represents an array with a fixed number of elements of different types.
- `function`: Represents a function type.

```jsx
// object
let person: object = { name: "Alice", age: 25 };

// function
function calculateTotalPrice(price: number, quantity: number): number {
    return price * quantity;
}

function logMessage(message: string): void {
    console.log(message);
}

const totalPrice = calculateTotalPrice(50, 2); // Returns 100
logMessage(`Total Price: ${totalPrice}`);
```

### 3. **Special Types**:

- `any`: Represents any type, allowing for flexibility but sacrifices type safety.
- `unknown`: A safer alternative to `any`, where you must perform type-checking before using it.
- `void`: Represents the absence of a value, typically used for functions that don’t return a value.
- `never`: Represents a value that never occurs (e.g., a function that always throws an error).
- `undefined` and `null`: Special types representing absence of a value.

```jsx
// 1. `any` - Allows any type, sacrifices type safety
let anything: any = "Hello";
anything = 42;  // No error, flexibility but no type safety

// 2. `unknown` - Safer than `any`, requires type checking
let unknownValue: unknown = "Hello";
if (typeof unknownValue === "string") {
  console.log(unknownValue.length);  // Safe after type checking
}

// 3. `void` - Represents the absence of a value (used for functions with no return value)
function logMessage(message: string): void {
  console.log(message);
}
let result: void = logMessage("Logging message");

// 4. `never` - Represents a value that never occurs (e.g., function that throws an error)
function throwError(message: string): never {
  throw new Error(message);  // Never returns a value
}

// 5. `undefined` and `null` - Represent absence of value
let uninitialized: undefined = undefined;  // Undefined variable
let emptyValue: null = null;  // Null value for intentional absence

console.log(uninitialized);  // Output: undefined
console.log(emptyValue);  // Output: null

let x;             // `x` is declared but not initialized, so it is undefined.
let y = null;      // `y` is explicitly set to null

console.log(x);    // Output: undefined
console.log(y);    // Output: null

// Checking types
console.log(typeof x);  // Output: "undefined"
console.log(typeof y);  // Output: "object"

// Comparing undefined and null
console.log(undefined == null);  // Output: true
console.log(undefined === null); // Output: false

```

### 4. **Advanced Types**:

- **Union Types** (`|`): Allows a value to be one of several types.
- **Intersection Types** (`&`): Combines multiple types into one.
- **Type Aliases** (`type`): Creates a custom type based on existing types.
- **Interfaces**: Describes the shape of an object, including properties and methods.

```jsx
interface Person {
  name: string;
}

interface Employee {
  salary: number;
}

type EmployeePerson = Person & Employee;  // Combines both interfaces

type StringOrNumber = string | number;
let identifier: StringOrNumber = "ID123";
identifier = 456;  // Valid
```

### Practice in Session :

**Problem**: Given the following code, predict the output and explain why.

```jsx
1. 
let a = "Hello";
a = 42;
console.log(a);

2.
function multiply(a: number, b: number): number {
  return a * b;
}

multiply(2, 3);  // Output or Error?
multiply(2, "3");  // Output or Error?

3.
function printData(data: any): void {
  console.log(data);
}

printData("Hello, world!");  // Output or Error?
printData(42);  // Output or Error?
printData([1, 2, 3]);  // Output or Error?
printData({ name: "Alice" });  // Output or Error?

4. 
let user = { name: "Alice", age: undefined };
console.log(user.age);  // Output or Error?

5. 
function logMessage(message: string): void {
  console.log(message);
}

let result: void = logMessage("Hello, TypeScript!");  // Output or Error?
console.log(result);  // Output or Error?

Note : 
let data: string = null;  // Error: Type 'null' is not assignable to type 'string' when strictNullChecks is enabled.

6.
function processData(data: unknown): void {
  if (typeof data === "string") {
    console.log(data.toUpperCase());  // Safe: data is a string
  } else {
    console.log("Not a string");
  }
}

processData("Hello");  // Output or Error?
processData(42);       // Output or Error?

7. 
// Using `any`
function getLengthAny(data: any): number {
  return data.length;  // Output or Error?
}

// Using `unknown`
function getLengthUnknown(data: unknown): number {
  if (typeof data === "string") {
    return data.length;  // Output or Error?
  }
  return 0;  // Output or Error?
}

getLengthAny("Hello");  // Output or Error?
getLengthAny(123);      // Output or Error?

getLengthUnknown("Hello");  // Output or Error?
getLengthUnknown(123);      // Output or Error?

```

### **TypeScript 101: Hands-on Project (No OOPs)**

In this **TypeScript 101** session, we’ll create a **simple interactive to-do list application** that demonstrates TypeScript's basic features like types, functions, `any`, `unknown`, `void`, `null`, `undefined`, and type inference.

### **Project: Simple To-Do List Application**

We will:

1. Define types for a to-do list.
2. Write functions to add, remove, and display to-do items.
3. Use `any` and `unknown` types where necessary to showcase flexibility and safety.
4. Handle cases of `null` and `undefined` safely.

```jsx
// Define types for a to-do list item
type TodoItem = {
    id: number;
    task: string;
    completed: boolean;
};

// Create an array to hold to-do items
let todoList: TodoItem[] = [];

// Function to add a new to-do item
function addTodo(task: string): void {
    const newTodo: TodoItem = {
        id: todoList.length + 1,  // Simple auto-increment for ID
        task: task,
        completed: false,
    };
    todoList.push(newTodo);
    console.log(`Task added: ${task}`);
}

// Function to remove a to-do item by ID
function removeTodo(id: number): void {
    const index = todoList.findIndex(todo => todo.id === id);
    if (index !== -1) {
        const removedTodo = todoList.splice(index, 1);
        console.log(`Task removed: ${removedTodo[0].task}`);
    } else {
        console.log('Task not found!');
    }
}

// Function to display the current to-do list
function displayTodos(): void {
    if (todoList.length === 0) {
        console.log('No tasks to show.');
        return;
    }

    todoList.forEach(todo => {
        const status = todo.completed ? 'Completed' : 'Pending';
        console.log(`ID: ${todo.id}, Task: ${todo.task}, Status: ${status}`);
    });
}

// Function to mark a to-do item as completed
function markAsCompleted(id: number): void {
    const todo = todoList.find(todo => todo.id === id);
    if (todo) {
        todo.completed = true;
        console.log(`Task completed: ${todo.task}`);
    } else {
        console.log('Task not found!');
    }
}

// Example usage:
addTodo("Learn TypeScript");
addTodo("Write Unit Tests");
displayTodos();

markAsCompleted(1);
displayTodos();

removeTodo(2);
displayTodos();

```

### Notes : How a .ts file is compiled?

- tsc will transpile the x.ts file and overwrite the code written in x.js
- transpile?
- compile : high level to machine code [happen in RAM]
- interpretor : execute machine code [happen in CPU]
- nodeJS cannot compile .ts