# Designing a Tic Tac Toe Game

This article explores the design and implementation of a Tic Tac Toe game using OOPs in Python. 

## System Requirements

The Tic Tac Toe game will:

1. **Handle Player Moves:** Allow P players to alternately place their marks/symbols (X, O, etc.) on a NxN grid.
2. **Check for Win or Draw:** Determine the outcome of the game – a win for one player, a draw, or continuation.
3. **Reset the Game:** Enable starting a new game at any point in time.

## Core Use Cases

1. **Making a Move:** Players take turns to place their marks on the grid.
2. **Checking Game Status:** After each move, check if the game is won, drawn, or still ongoing.
3. **Resetting the Game:** Clear the board for a new round.

## Key Classes:
- `TicTacToe`: Manages the overall game.
- `Board`: Represents the game board.
- `Player`: Class to represent the players and their marks.

## Python Implementation

### Player Class
Represents the players in the game.

```python
class Player:
    def __init__(self, name, symbol):
        self.name = name
        self.symbol = symbol
```

### Board Class
Represents the game board.

```python
class Board:
    def __init__(self, size):
        self.size = size
        self.markersPlaced = 0
        self.board = []
        for i in range(size):
            self.board([""] * size)
        self.rowData = {}
        self.colData = {}
        self.diagData = {0: {}, 1: {}}

    def isFull(self):
        return self.markersPlaced == self.size * self.size

    def resetBoard(self):
        self.size = size
        self.markersPlaced = 0
        self.board = []
        for i in range(size):
            self.board([""] * size)
        self.rowData = {}
        self.colData = {}
        self.diagData = {0: {}, 1: {}}

    def makeMove(self, player, row, col):
        if self.board[row][col] != "":
            raise ValueError("Not an empty space")

        self.board[row][col] = player.symbol
        self.markersPlaced += 1
        result = False

        if row not in self.rowData.keys():
            self.rowData[row] = {}
        if player.symbol not in self.rowData[row].keys():
            self.rowData[row][player.symbol] = 0
        self.rowData[row][player.symbol] += 1
        if self.rowData[row][player.symbol] == self.size:
            result = True

        if col not in self.colData.keys():
            self.colData[col] = {}
        if player.symbol not in self.colData[col].keys():
            self.colData[col][player.symbol] = 0
        self.colData[col][player.symbol] += 1
        if self.colData[col][player.symbol] == self.size:
            result = True

        if row == col:
            if player.symbol not in self.diagData[0].keys():
                self.diagData[0][player.symbol] = 0
            self.diagData[0][player.symbol] += 1
        if self.diagData[0][player.symbol] == self.size:
            result = True

        if row + col == self.size - 1:
            if player.symbol not in self.diagData[1].keys():
                self.diagData[1][player.symbol] = 0
            self.diagData[1][player.symbol] += 1
        if self.diagData[1][player.symbol] == self.size:
            result = True

        return result
```
### TicTacToe Class
```python
from enum import Enum

class GameState(Enum):
    NOT_STARTED = 1
    IN_PROGRESS = 2
    WIN = 3
    TIE = 4

class TicTacToe:
    def __init__(self):
        self.gameState = GameState.NOT_STARTED

    def play(self):
        gameBoard = Board(3)
        player1 = Player("A", "X")
        player2 = Player("B", "O")
        self.gameState = IN_PROGRESS
        while self.gameState == IN_PROGRESS:
            if gameBoard.isFull():
                self.gameState = GameState.TIE
                break
        ...contd
```