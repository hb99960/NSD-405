# NSD S08 LLD : Interview Question : Parking Lot

## Problem Statement

A parking lot is an area where cars can be parked for a certain amount of time. A parking lot can have multiple floors with each floor having a different number of slots and each slot being suitable for different types of vehicles. For this problem, we have to design the next generation parking lot system which can manage a parking lot without any human intervention.

### Requirements :

- The functions that the parking lot system can do:
    - Create the parking lot.
    - Add floors to the parking lot.
    - Add a parking lot slot to any of the floors.
    - Given a vehicle, it finds the first available slot, books it, creates a ticket, parks the vehicle, and finally returns the ticket.
    - Unparks a vehicle given the ticket id.
    - Displays the number of free slots per floor for a specific vehicle type.
    - Displays all the free slots per floor for a specific vehicle type.
    - Displays all the occupied slots per floor for a specific vehicle type.
- Details about the Vehicles:
    - Every vehicle will have a type, registration number, and color.
    - Different Types of Vehicles:
        - Car
        - Bike
        - Truck
- Details about the Parking Slots:
    - Each type of slot can park a specific type of vehicle.
    - No other vehicle should be allowed by the system.
    - Finding the first available slot should be based on:
        - The slot should be of the same type as the vehicle.
        - The slot should be on the lowest possible floor in the parking lot.
        - The slot should have the lowest possible slot number on the floor.
    - Numbered serially from 1 to n for each floor where n is the number of parking slots on that floor.
- Details about the Parking Lot Floors:
    - Numbered serially from 1 to n where n is the number of floors.
    - Might contain one or more parking lot slots of different types.
    - We will assume that the first slot on each floor will be for a truck, the next 2 for bikes, and all the other slots for cars.
- Details about the Tickets:
    - The ticket id would be of the following format:<parking_lot_id>_<floor_no>_<slot_no>Example: PR1234_2_5 (denotes 5th slot of 2nd floor of parking lot PR1234)
- We can assume that there will only be 1 parking lot. The ID of that parking lot is PR1234.

## Code Solution

```tsx
// index.ts
import fs from 'fs';
import { join } from 'path';
import readLine from 'readline';
import ParkingLotController from './controller/parking-lot-controller';
import CommandController from './controller/command-controller';

function readCommand() {
    const fileName = process.argv[2];
    const file = join(process.cwd(), fileName);
    if (!fs.existsSync(file)) {
        throw new Error(`${file} File not found`);
    }

    const rl = readLine.createInterface({
        input: fs.createReadStream(file)
    })
    const parkingLotController = new ParkingLotController();
    const commandController = new CommandController(parkingLotController);
    rl.on('line', (data) => {
        const [command, ...args] = data.split(' ');
        if (command === 'exit') {
            console.log('Exiting!!');
            return rl.close();
        }
        let resp = '';
        try {
            resp = commandController.executeCommand(command, ...args);
        } catch (e) {
            resp = 'Unknow Error';
            if (e instanceof Error) {
                resp = e.message
            }
        } finally {
            console.log('RESPONSE>>', resp);
        }
        
    });
}

readCommand();
```

types.js

```tsx
enum VehicleType {
    BIKE = 'BIKE',
    CAR = 'CAR',
    TRUCK = 'TRUCK'
}

interface IVehicle {
    type: VehicleType;
    regNo: string;
    color: string;
}

interface ISlot {
    id: number;
    type: VehicleType;
    isOccupied: boolean;
    floorId:number;
    occupy: () => void;
    release: () => void;
}

interface IFloor {
    id: number;
    slots: Array<ISlot>;
    addSlot: (vehicleType: VehicleType) => void;
    // occupySlot: (vehicleType: VehicleType) => boolean | number;
    getAvailableSlots: (vehicleType?: VehicleType) => Array<ISlot>
    getOccupiedSlots: (vehicleType?: VehicleType) => Array<ISlot>
}

interface ITicket {
    id: string;
    vehicle: IVehicle;
}

interface IParkingStrategy {
    park(floors: IFloor[], vehicle: IVehicle): ISlot | null;
}

export { VehicleType, IVehicle, ISlot, IFloor, ITicket, IParkingStrategy };
```

command-controller.ts

```tsx
import ParkingLotController from "./parking-lot-controller";

class CommandController {
    parkingLotController: ParkingLotController;

    constructor(parkingLotController: ParkingLotController) {
        this.parkingLotController = parkingLotController;
    }

    executeCommand(command: string, ...args: Array<string | number>) {
        console.log('\nCOMMAND>>', command, args, '\n');
        const { parkingLotController } = this;
        switch(command) {
            case 'create_parking_lot':
                return parkingLotController.createParkingLot(...args as [string, number, number]);
            case 'park_vehicle':
                return parkingLotController.parkVehicle(...args as [string, string, string]);
            case 'unpark_vehicle':
                return parkingLotController.unparkVehicle(...args as [string]);
            case 'display':
                return parkingLotController.display(...args as [string, string]);
            default: 
                return 'Invalid Command';
        }
    }
}

export default CommandController;
```

parking-lot-controller.ts

```tsx
import ParkingLot from "../model/parking-lot";
import { ISlot, VehicleType } from "../types";
import Vehicle from "../model/vehicle";
import { RandomParkingStrategy } from "../model/RandomParkingStrategy";

class ParkingLotController {
    parkingLot!: ParkingLot

    createParkingLot(id: string, totalFloors: number, totalSlots: number) {
        if (this.parkingLot) {
            throw new Error('Parking Lot already exists');
        }
        this.parkingLot = new ParkingLot(id, new RandomParkingStrategy);
        this.parkingLot.addFloors(totalFloors);

        const floors = this.parkingLot.floors;
        floors.forEach(floor => {
            for (let ind = 0; ind < totalSlots; ind++) {
                const vehicleType = ind === 0 ? VehicleType.TRUCK : ind <= 2 ? VehicleType.BIKE : VehicleType.CAR;
                floor.addSlot(vehicleType);
            }
        });
        return `Created parking lot with ${totalFloors} floors and ${totalSlots} slots per floor`;
    }

    parkVehicle(vType: string, regNo: string, color: string) {
        const vehicleType = VehicleType[vType as keyof typeof VehicleType];
        const vehicle = new Vehicle(vehicleType, regNo, color);
        const ticket = this.parkingLot.parkVehicle(vehicle);
        if (ticket) {
            // this.tickets.push(ticket);
            return `Parked vehicle. Ticket ID: ${ticket.id}`;
        }
        return "Parking Lot Full";
    }

    unparkVehicle(ticketId: string) {
        return this.parkingLot.unparkVehicle(ticketId);
    }

    display(displayType: string, vType: string) {
        // console.log(displayType, vType);
        const vehicleType = VehicleType[vType as keyof typeof VehicleType];
        // console.log(vehicleType);
        let data;
        switch(displayType) {
            case 'free_slots':
            case 'free_count':
                data = this.parkingLot.getFreeSlots(vehicleType, displayType === 'free_slots');
                break;
            case 'occupied_slots':
                data = this.parkingLot.getOccupiedSlots(vehicleType);
                break;
        }

        let resp = '';
        if (data) {
            Object.entries(data).forEach(d => {
                const [floor, slotsOrCount] = d;
                resp += printSlotsData(displayType, vehicleType, floor, slotsOrCount) + '\n';
            })
        }
        return resp;
    }
}

function printSlotsData(displayType: string, vehicleType: string, floor: string, slotsOrCount?: Array<ISlot> | number) {
    switch(displayType) {
        case 'free_slots' :
            return `No. of free slots for ${vehicleType} on Floor ${floor}: ${(slotsOrCount as Array<ISlot>).map(slot => slot.id)}`;
        case 'free_count' :
            return `Free slots for ${vehicleType} on Floor ${floor}: ${slotsOrCount}`;
        case 'occupied_slots' :
            return `Occupied slots for ${vehicleType} on Floor ${floor}: ${(slotsOrCount as Array<ISlot>).map(slot => slot.id)}`;
    }
}

export default ParkingLotController;
```

ticket-controller.ts

```tsx
import Ticket from "../model/ticket";
import { ITicket, IVehicle } from "../types";

class TicketController {
    // Private static variable to hold the single instance
    private static instance: TicketController | null = null;

    // Static Map to store tickets
    static tickets = new Map<string, ITicket>();

    // Private constructor to prevent direct instantiation
    private constructor() {}

    // Public static method to get the instance of TicketController (Singleton)
    public static getInstance(): TicketController {
        if (this.instance === null) {
            this.instance = new TicketController();
        }
        return this.instance;
    }

    static getTicketWithId(ticketId: string): ITicket | undefined {
        return TicketController.tickets.get(ticketId);
    }

    // Method to delete a ticket by ID
    static deleteTicket(ticketId: string) {
        return TicketController.tickets.delete(ticketId);
    }

    // Method to generate a new ticket and store it
    static generateTicket(lotId: string, slotId: string | number, floorId: string | number, vehicle: IVehicle) {
        const ticketId = `${lotId}_${floorId}_${slotId}`;
        const ticket = new Ticket(ticketId, vehicle);
        TicketController.tickets.set(ticketId, ticket);
        return ticket;
    }
}

export default TicketController;
```

DefaultParkingStrategy.ts

```tsx

import { IFloor, IParkingStrategy, ISlot, IVehicle } from "../types";

export class DefaultParkingStrategy implements IParkingStrategy {
  park(floors: IFloor[], vehicle: IVehicle): ISlot | null {
    for (let floor of floors) {
      const [slotToBook] = floor.getAvailableSlots(vehicle.type);
      if (slotToBook) {
        return slotToBook;
      }
    }
    return null;
  }
}
```

RandomParkingStrategy.ts

```tsx
// random-parking-strategy.ts

import { IParkingStrategy, IVehicle } from "../types";
import { IFloor, ISlot } from "../types";

export class RandomParkingStrategy implements IParkingStrategy {
  park(floors: IFloor[], vehicle: IVehicle): ISlot | null {
    const availableSlots: ISlot[] = [];

    // Collect all available slots
    floors.forEach((floor) => {
      availableSlots.push(...floor.getAvailableSlots(vehicle.type));
    });

    if (availableSlots.length === 0) return null;

    // Select a random slot
    const randomSlot = availableSlots[Math.floor(Math.random() * availableSlots.length)];
    return randomSlot;
  }
}
```

floor.ts

```tsx
import Slot from "./slot";
import { IFloor, ISlot, VehicleType } from "../types";

class Floor implements IFloor {
    id: number;
    slots: Array<ISlot>

    constructor(id: number) {
        this.id = id;
        this.slots = [];
    }

    addSlot(vehicleType: VehicleType) {
        this.slots.push(new Slot(this.slots.length + 1, vehicleType, this.id));
    }

    getAvailableSlots(vehicleType?: VehicleType) {
        let availableSlots = [];
        for (const slot of this.slots) {
            if (slot.isOccupied) {
                continue;
            }

            if (!vehicleType) {
                availableSlots.push(slot)
            } else if (slot.type === vehicleType) {
                availableSlots.push(slot);
            }
        }
        // console.log(availableSlots);
        return availableSlots;
    }

    getOccupiedSlots(vehicleType?: VehicleType) {
        let occupiedSlots = [];
        for (const slot of this.slots) {
            if (!slot.isOccupied) {
                continue;
            }

            if (!vehicleType) {
                occupiedSlots.push(slot)
            } else if (slot.type === vehicleType) {
                occupiedSlots.push(slot);
            }
        }
        return occupiedSlots;
    }
}

export default Floor;
```

parking-lot.ts

```tsx

import Floor from "./floor";
import { IFloor, IParkingStrategy, ISlot, IVehicle, VehicleType } from "../types";
import TicketController from "../controller/ticket-controller";

class ParkingLot {
    id: string;
    floors: Array<IFloor>;
    private parkingStrategy: IParkingStrategy;

    constructor(id: string, parkingStrategy:IParkingStrategy) {
        this.id = id;
        this.floors = [];
        this.parkingStrategy = parkingStrategy;
    }

    setParkingStrategy(parkingStrategy:IParkingStrategy){
        this.parkingStrategy = parkingStrategy;
    }

    addFloors(floorsToAdd: number) {
        for (let i = 1; i <= floorsToAdd; i++) {
            this.floors.push(new Floor(i));
        }
    }

    addSlot(floorNumber: number, vehicleType: VehicleType) {
        const floor = this.floors[floorNumber];
        floor.addSlot(vehicleType);
    }

    parkVehicle(vehicle: IVehicle) {
        
        const slotToBook = this.parkingStrategy.park(this.floors, vehicle);
    
        if (slotToBook) {
            slotToBook.occupy();
            const ticket = TicketController.generateTicket(this.id, slotToBook.id, slotToBook.floorId, vehicle);
            return ticket;
        }

        throw new Error('Parking Lot Full');
    }

    getFreeSlots(vehicleType: VehicleType, showSlots: boolean = false) {
        const floors = this.floors;
        const freeSlots: { [x: string]: ISlot[] | number } = {};
        
        floors.forEach(floor => {
            const availableSlots = floor.getAvailableSlots(vehicleType);
            // console.log({ availableSlots });
            freeSlots[floor.id] = showSlots ? availableSlots : availableSlots.length;
        });
        // console.log({ freeSlots });
        return freeSlots;
    }

    getOccupiedSlots(vehicleType: VehicleType) {
        const floors = this.floors;
        const occupiedSlots: { [x: string]: ISlot[] | number } = {};
        
        floors.forEach(floor => {
            occupiedSlots[floor.id] = floor.getOccupiedSlots(vehicleType);
        });
        return occupiedSlots;
    }

    unparkVehicle(ticketId: string) {
        const ticket = TicketController.getTicketWithId(ticketId);

        if (ticket) {            
            const [_parkingLotId, floorId, slotId] = ticketId.split('_');        

            const floor = this.floors[Number(floorId) - 1];
            const slot = floor.slots[Number(slotId) - 1];
            slot.release();
            TicketController.deleteTicket(ticketId);
            return `Unparked vehicle with Registration Number: ${ticket.vehicle.regNo} and Color: ${ticket.vehicle.color}`;
        }

        throw new Error('Invalid Ticket');
    }
}

export default ParkingLot;
```

slot.ts

```tsx
import { VehicleType, ISlot } from "../types";

class Slot implements ISlot {
    id: number;
    type: VehicleType;
    isOccupied: boolean;
    floorId: number;

    constructor(id: number, type: VehicleType, floorId: number) {
        this.id = id;
        this.type = type;
        this.isOccupied = false;
        this.floorId = floorId;
    }

    occupy() {
        if (this.isOccupied) {
            throw new Error('Already Occupied');
        }
        this.isOccupied = true;
    }

    release() {
        if (!this.isOccupied) {
            throw new Error('Already free');
        }
        this.isOccupied = false;
    }
}

export default Slot;
```

ticket.ts

```tsx
import { ITicket, IVehicle } from "../types";

class Ticket implements ITicket {
    id: string;
    vehicle: IVehicle;

    constructor(id: string, vehicle: IVehicle) {
        this.id = id;
        this.vehicle = vehicle;
    }
}

export default Ticket;
```

vehicle.ts

```tsx
import { IVehicle, VehicleType } from "../types";

class Vehicle implements IVehicle {
    type: VehicleType;
    regNo: string;
    color: string;

    constructor(type: VehicleType, regNo: string, color: string) {
        this.type = type;
        this.regNo = regNo;
        this.color = color;
    }
}

export default Vehicle;
```

commands.txt

```
create_parking_lot PL123 2 6
park_vehicle CAR KA-01-DB-1234 black
display free_count CAR
unpark_vehicle PR1234_2_5
display free_count BIKE
exit
```

---

## Output

![Screenshot 2025-02-19 at 2.41.33 PM.png](NSD%20S08%20LLD%20Interview%20Question%20Parking%20Lot%20193a26964f56808f8e4ae40e7e1a31ad/Screenshot_2025-02-19_at_2.41.33_PM.png)