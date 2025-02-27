# NSD S12 : Interview LLD Questions : Tic-Tac-Toe

## Problem Statement

### **Objective**

Develop a **console-based Tic-Tac-Toe game** where two players take turns marking cells on a **3x3 grid**. The game should **validate moves**, **detect winners**, and allow players to choose their **names** and **symbols**.

---

### **Functional Requirements**

### **1. Game Board Setup**

- The board is a **3x3 grid**, represented using rows labeled `A, B, C` and columns numbered `1, 2, 3`.
- The board should **initialize with empty slots (`_`)** and update after each move.

### **2. Player Registration**

- Two players must **enter their names** before the game starts.
- Each player chooses a **unique character** (e.g., `X`, `O`) for marking their moves.
- The game **does not allow `_` as a player character**.

### **3. Gameplay Mechanics**

- Players take turns **entering a box identifier** (e.g., `A1`, `B2`) to place their symbol.
- If a player **tries to mark an already occupied cell**, the move is rejected.
- The game keeps track of **turns**, alternating between Player 1 and Player 2.

### **4. Win Conditions**

- A player **wins** if they have **three of their marks** in:
    - Any **row** (e.g., `XXX` in row `A`).
    - Any **column** (e.g., `OOO` in column `2`).
    - Any **diagonal** (e.g., `XOOOX` in main diagonal).
- When a player wins, the game **announces the winner** and **ends immediately**.

### **5. Draw Condition**

- If **all 9 cells are filled** and no player has won, the game declares a **draw**.

### **6. Error Handling & Validation**

- If an **invalid box** (e.g., `Z5`) is entered, an **error is thrown**.
- If a **row or column number is out of bounds**, an **error is thrown**.
- If an **invalid player character** is chosen, an **error is thrown**.

---

### **Constraints & Edge Cases**

- The **board size is fixed** at `3x3`.
- The **game does not allow undoing a move**.
- If players **select the same character**, both must choose again.
- The game should be **console-based** (no GUI implementation required).

## Code Solution

```tsx
class InvalidBoxNameError extends Error {
    message = 'Invalid box identifier';
}

const BOARD_DEFAULT_SIZE = 3
const ROWS = "ABCDEFGHIJ"

export class Board {
    size!: number
    grid!: Array<Array<string>>

    constructor(size: number = BOARD_DEFAULT_SIZE) {
        this.size = size
        this.grid = []
        for (let i = 0; i < size; i++) {
            this.grid.push(new Array(size).fill("_"))
        }
    }

    getBoardForDisplay(): string {
        const displayRows = []
        for (let row of this.grid) {
            displayRows.push(row.join('\t'))
        }
        return displayRows.join('\n')
    }

    markBoard(box: string, character: string): boolean {
        // box: A1 or B2 like that
        if (box.length != 2) {
            throw new InvalidBoxNameError()
        }
        const row = ROWS.indexOf(box.charAt(0))
        const col = Number(box.charAt(1)) - 1

        if (row < 0 || row >= this.size || col < 0 || col >= this.size) {
            throw new InvalidBoxNameError()
        }

        if (this.grid[row][col] != "_") {
            return false
        }

        this.grid[row][col] = character
        return true
    }

    getRowAsString(rowName: string): string {
        const row = ROWS.indexOf(rowName)
        if (row == -1 || row >= this.size) {
            throw new Error(`row number is invalid`)
        }
        return this.grid[row].join("")
    }

    getColAsString(col: number): string {
        if (col < 0 || col > this.size) {
            throw new Error("col has to be between 0 and 2")
        }
        const colVals = []
        for (let i = 0; i < this.size; i++) {
            colVals.push(this.grid[i][col])
        }

        return colVals.join("")
    }

    getDiagAsString(diagNo: number): string {
        const diagVals = []
        if (diagNo == 0) {
            for (let i = 0; i < this.size; i++) {
                diagVals.push(this.grid[i][i])
            }

            return diagVals.join("")
        } else if (diagNo == 1) {

            for (let i = 0; i < this.size; i++) {
                diagVals.push(this.grid[i][this.size - 1 - i])
            }
            return diagVals.join("")
        } else {
            throw new Error("Invalid diagonal")
        }
    }

}
```

```tsx
import { Board } from "./Board"
import { Player } from "./Player"
type GameState = "STARTED" | "END_WINNER" | "END_DRAW"

export class Game {
    p1: Player
    p2: Player
    board: Board
    turn = 0
    state: GameState = "STARTED"

    private constructor(p1: Player, p2: Player, board: Board) {
        this.p1 = p1;
        this.p2 = p2;
        this.board = board;
    }

    private checkWinner(player: Player): boolean {
        const c = player.character
        const winningLine = `${c}${c}${c}`
        for (let row of ["A", "B", "C"]) {
            if (this.board.getRowAsString(row) == winningLine)
                return true
        }
        for (let col of [0,1,2]) {
            if (this.board.getColAsString(col) == winningLine)
                return true
        }
        for (let diag of [0,1]) {
            if (this.board.getDiagAsString(diag) == winningLine)
                return true
        }

        return false

    }

    nextTurnPrompt(): string {
        const player = this.turn % 2 == 0 ? this.p1 : this.p2;

        return '\n' + this.board.getBoardForDisplay()
            + '\n'
            + `Turn: ${this.turn + 1}  |  Player : ${player.name} (${player.character})`
    }

    play(box: string) {
        const player = this.turn % 2 == 0 ? this.p1 : this.p2;

        const success = this.board.markBoard(box, player.character)

        if (success) {
            if (this.checkWinner(player)) {
                this.state = "END_WINNER"
                console.log(`Game Over! ${player.name} has won!`)
                return
            }
            this.turn++;
        }

        if (this.turn == 9) {
            this.state = "END_DRAW"
            console.log("Game ended in DRAW")
        }
    }

    static Builder = class GameBuilder {
        p1!: Player
        p2!: Player

        addPlayer1(name: string, character: string = "X"): GameBuilder {
            this.p1 = new Player.Builder()
                .setName(name)
                .setCharacter(character)
                .build()
            return this;
        }

        addPlayer2(name: string, character: string = "X"): GameBuilder {
            this.p2 = new Player.Builder()
                .setName(name)
                .setCharacter(character)
                .build()
            return this;
        }

        build(): Game {
            if (!this.p1) {
                throw new Error("You need to create player 1 before building game")
            }

            if (!this.p2) {
                throw new Error("You need to create player 2 before building game")
            }

            return new Game(this.p1, this.p2, new Board())
        }
    }

}
```

```tsx
export class Player {
    name: string;
    character: string;
    static Builder = class PlayerBuilder {
        private name!: string;
        private character!: string;

        setName(value: string): PlayerBuilder {
            this.name = value;
            return this;
        }

        setCharacter(value: string): PlayerBuilder {
            if (value == "_") {
                throw new Error("Underscore '_' is not a valid player character")
            }
            this.character = value;
            return this;
        }

        build(): Player {
            return new Player(this.name, this.character)
        }
    }

    private constructor(name: string, character: string) {
        this.name = name;
        this.character = character;
    }
}
```

```tsx
import { Game } from "./Game";
import * as rl from "readline-sync";

const gameBuilder = new Game.Builder()

const player1Name = rl.question("Enter Player 1 name: ")
const player1Char = rl.question("Enter Player 1 character (X): ", { defaultInput: "X" })

gameBuilder.addPlayer1(player1Name, player1Char)

const player2Name = rl.question("Enter Player 2 name: ")
const player2Char = rl.question("Enter Player 2 character (O): : ", { defaultInput: "O" })

gameBuilder.addPlayer2(player2Name, player2Char)

const game = gameBuilder.build()

while (game.state == "STARTED") {
    console.log(game.nextTurnPrompt())
    const box = rl.question("Enter Box: ")
    game.play(box)
}
```