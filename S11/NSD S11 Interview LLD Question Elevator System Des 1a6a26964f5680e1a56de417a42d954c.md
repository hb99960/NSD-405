# NSD S11 : Interview LLD Question : Elevator System Design

## Problem Statement : **Elevator Management System**

## **Objective**

Design and implement a **smart elevator control system** that efficiently manages multiple elevators in a building. The system should handle elevator requests from different floors, determine the best elevator to fulfill a request, and simulate elevator movements, door operations, and state transitions.

---

## **Functional Requirements**

### **Building & Elevator Setup**

- The system must support **multiple floors** and **multiple elevators**.
- Each elevator should start at **floor 1** and be able to move up or down.

### **Elevator Requests & Assignment**

- A **passenger can request an elevator** from any floor, specifying the desired destination.
- The system should determine the **best elevator** to serve a request based on:
    - The elevator’s current position.
    - The direction of travel (up or down).
    - The elevator’s idle status.

### **Elevator State Management**

- An elevator operates in **three states**:
    - **MovingState** → The elevator is in transit.
    - **OpenDoor** → The doors are open at a floor.
    - **CloseDoor** → The doors are closed, and the elevator is ready for movement.

### **Elevator Movement**

- The assigned elevator moves **one floor at a time** toward the requested floor.
- The elevator must **open its doors upon arrival** and close them after a delay.
- If an elevator is already moving in the **requested direction**, it should prioritize requests in the same direction.

### **Display & Monitoring**

- Each elevator should provide real-time updates:
    - Current floor.
    - Direction of movement.
    - Occupancy status.

---

## **Constraints & Edge Cases**

- If all elevators are occupied or moving in the opposite direction, **the request should be logged** for later assignment.
- If an elevator **is already at the requested floor**, doors should open immediately.
- Doors **should not open while the elevator is moving**.
- The system should handle **unexpected requests gracefully**.

## Code Solution :

```tsx
// Paste the entire code provided for the elevator system here
import { Building } from "./Building";
import { ElevatorRequest } from "./ElevatorRequest";

// Create a building with 10 floors and 2 elevators
const building = new Building(10, 2);

// Test requesting an elevator from floor 3 going up
const request1 = new ElevatorRequest(3, 7); // From floor 3 to floor 7
const elevator1 = building.requestElevator(request1);

// Test requesting an elevator from floor 5 going down
const request2 = new ElevatorRequest(5, 2); // From floor 5 to floor 2
const elevator2 = building.requestElevator(request2);

// Update displays after requests
// building.updateDisplays();

// Assuming elevators have been assigned from Scenario 1

// Simulate elevator movements and operations
if (elevator1) {
    // Move elevator 1 to request 1's current floor
    elevator1.moveToFloor(request1.currentFloor);
    elevator1.openDoor(); // Open door at request 1's floor
}

if (elevator2) {
    // Move elevator 2 to request 2's current floor
    elevator2.moveToFloor(request2.currentFloor);
    elevator2.openDoor(); // Open door at request 2's floor
}

// Close doors after a delay
setTimeout(() => {
    if (elevator1) {
        elevator1.closeDoor();
    }
    if (elevator2) {
        elevator2.closeDoor();
    }
}, 5000); // Close doors after 5 seconds

// Update displays after operations
building.updateDisplays();
```

```tsx
import { ElevatorRequest } from "./ElevatorRequest";
import { FloorPanel } from "./FloorPanel";
import { InsideControlPanel } from "./InsideControlPanel";
import { OutsideControlPanel } from "./OutsideControlPanel";
import { Direction } from "./ElevatorRequest";
import { Elevator2 } from "./IElevatorState";

export class Building {
    numFloors: number;
    elevators: Elevator2[];
    

    constructor(numFloors: number, numElevators: number) {
        this.numFloors = numFloors;
        this.elevators = [];
       

        // Initialize elevators
        for (let i = 0; i < numElevators; i++) {
            this.elevators.push(new Elevator2());
        }
    }

    updateDisplays(): void {
        for (const elevator of this.elevators) {
            elevator.updateDisplays();
        }
    }

    requestElevator(request: ElevatorRequest): Elevator2 | null {
        let bestElevator: Elevator2 | null = null;
        let minDistance = Infinity;

        for (const elevator of this.elevators) {
            const distance = Math.abs(elevator.currentFloor - request.currentFloor);
            if (elevator.direction === request.direction || elevator.direction === Direction.Idle) {
                if (distance < minDistance) {
                    minDistance = distance;
                    bestElevator = elevator;
                }
            }
        }

        if (bestElevator) {
            bestElevator.moveToFloor(request.currentFloor);
        } else {
            console.log('No available elevator to fulfill the request.');
        }

        return bestElevator;
    }
}
```

```tsx
export enum Direction {
    Up = 'up',
    Down = 'down',
    Idle = 'idle'
}
export class ElevatorRequest {
    currentFloor: number;
    destinationFloor: number;
    direction: Direction;

    constructor(currentFloor: number, destinationFloor: number) {
        this.currentFloor = currentFloor;
        this.destinationFloor = destinationFloor;
        this.direction = this.destinationFloor > this.currentFloor ? Direction.Up : Direction.Down;
    }
}
```

```tsx
interface ElevatorState{
    moveToFloor(elevator: Elevator2, floor: number):void;
    openDoor(elevator: Elevator2):void;
    closeDoor(elevator: Elevator2):void;
}
```

```tsx
export class CloseDoor implements ElevatorState{
    moveToFloor(elevator: Elevator2, floor: number): void {

        console.log(`Elevator is moving to floor ${floor}`);
        elevator.direction = floor > elevator.currentFloor ? Direction.Up : Direction.Down;
        elevator.state = new MovingState(); // Transition to Moving state

        let distance = Math.abs(floor - elevator.currentFloor);
        let step = elevator.currentFloor < floor ? 1 : -1; // Determine direction

        for (let i = 1; i <= distance; i++) {
            
                elevator.currentFloor += step;
                console.log(`Elevator at floor: ${elevator.currentFloor}`);

                if (elevator.currentFloor === floor) {
                    console.log(`Elevator has arrived at floor ${floor}`);
                    elevator.state = new CloseDoor(); // Transition back to Idle state
                    elevator.updateDisplays();
                }
           
        }

        elevator.currentFloor = floor;
        elevator.updateDisplays();
    }
    openDoor(elevator: Elevator2): void {
        console.log("Elevator doors opening...");
        elevator.doorOpen = true;
    }
    closeDoor(elevator: Elevator2): void {
        console.log("Elevator doors closing...");
        elevator.doorOpen = false;
    }
}

export class MovingState implements ElevatorState{
    moveToFloor(elevator: Elevator2, floor: number): void {
        console.log(`Already moving to floor ${floor}`);
    }
    openDoor(elevator: Elevator2): void {
        console.log("Cannot open doors while moving!");
    }
    closeDoor(elevator: Elevator2): void {
        console.log("Doors are already closed while moving.");
    }

}

export class OpenDoor implements ElevatorState{
    moveToFloor(elevator: Elevator2, floor: number): void {
        console.log("Cannot move while doors are open!");
    }
    openDoor(elevator: Elevator2): void {
        console.log("Doors are already open.");
    }
    closeDoor(elevator: Elevator2): void {
        console.log("Closing doors...");
        elevator.doorOpen = false;
        elevator.state = new CloseDoor(); // Transition to Closed Door state
    }
}
```

```tsx
export class Elevator2{
    currentFloor: number;
    direction: Direction;
    doorOpen: boolean;
    capacity: number;
    requests: number;
    maxCapacity: number = 8; // 8 people or 680 kilograms
    state: ElevatorState = new CloseDoor(); // Default state

    constructor(){
        this.currentFloor = 1; // Starting from floor 1
        this.direction = Direction.Idle;
        this.doorOpen = false;
        this.capacity = 0;
        this.requests = 0;
    }

    moveToFloor(floor: number): void {
        this.state.moveToFloor(this, floor);
    }

    openDoor(): void {
        this.state.openDoor(this);
    }

    closeDoor(): void {
        this.state.closeDoor(this);
    }

    updateDisplays(): void {
        console.log(`Current floor: ${this.currentFloor}, Direction: ${this.direction}, Capacity: ${this.requests}/${this.maxCapacity}`);
    }
}
```

```tsx
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