# >>>Final Project: Minesweeper
Create a program to play a text-based *Minesweeper*, a single player logic game.

### Table of Contents
- [Rules of Minesweeper](#rules-of-minesweeper)
- [Minimum Requirements](#minimum-requirements)
- [Sample Input/Output](#sample-inputoutput)
- [Tips](#tips)

[Feedback](#feedback) \
[License](#license)

## Rules of Minesweeper
In Minesweeper, the player tries to dig out a grid of cells without digging a mine. Dug cells provide information.

The game is played on a rectangular N by M **grid**, consisting of N*M **cells**. There are a known number of mines hidden in the grid. \
A cell is either **safe** or contains a mine. Mines are placed randomly each game. \
At the start of the game, all cells are **hidden** (undug) and the player cannot see which are mines. The player reveals (digs) cells they think are safe. \
**Revealed** cells show how many of their 8 neighbouring cells are mines, a number from `1` to `8`. (`0` is usually shown as blank instead of as `0`). Using this information, the player can deduce which cells are mines. Then, the player can determine which cells are not mines and dig them, providing more information.

Players can place **flags** on hidden cells that they think are mines, to help keep track of them. These are purely for the player's benefit: they are not required, and flags can potentially be placed incorrectly. A player can remove placed flags.

If a player digs a mine, they instantly lose. \
The player wins when all safe cells in the grid are revealed. (Note: flags are irrelevant for this.)

In almost all Minesweeper implementations, when a player digs a cell with 0 mine neighbours, all of its neighbours are dug automatically. This can chain, so large chunks of the grid may be cleared with a single dig.

Use https://www.google.com/fbx?fbx=minesweeper as reference (note that certain features of it aren't required). Left click to dig, right click or ctrl + left click to flag. \
Play a few games to familiarize yourself.

Two basic rules for solving:
- If the number of hidden neighbours of a cell is equal to its number, then the hidden neighbours are all mines and should be flagged
- If the number of flagged neighbours of a cell is equal to its number, then the remaining unflagged hidden neighbours are safe to dig

These two rules are actually enough to solve most grids.

Note: Some grids are not guaranteed to be solvable without guessing - it may generate in such a way that there is not enough information to deduce where the mines are in a specific spot. \
Additionally, in some implementations, it is possible for the player to hit a mine and lose the game on the very first dig.

## Minimum Requirements
- The program correctly follows the mechanics of Minesweeper during almost all gameplay
- The program displays the grid, and the player can input cell coordinates and choose to either reveal or flag the cell
- The program automatically and instantly clears neighbours of dug cells with 0 neighbouring mines, recursively
- The player can flag only hidden cells. The player can remove flags from flagged cells
- If the player digs a mine, the program tells them they lost, and the game ends
- When the player has revealed all safe cells, the program tells them they won, and the game ends
- When the game ends, the player may choose to either play again or exit the program
- The grid is 9x9, with 10 mines, and the player is told this at the start of the game. (Customizable games also fills this requirement)
- The grid mine placement is randomly generated
- In the codebase, all *code* is written by you: AI, tutorials, existing codebases, and other people cannot provide any of the code, though they may provide information.
    - Do not use any programming materials that are directly related to Minesweeper, such as Minesweeper codebases or "How to code Minesweeper" tutorials

## Sample Input/Output
A good grid display (that you may mimic) is this:
```
    1 2 3 4 5 6 7 8 9
  +-------------------+
1 | # F 1     2 F # # |
2 | 1 1 1     2 F # # |
3 |           1 2 # # |
4 |             1 # # |
5 |       1 1 2 2 # # |
6 |       1 F 2 # # # |
7 |     1 2 3 # # # # |
8 | 1 1 1 # # # # # # |
9 | # # # # # # # # # |
  +-------------------+
```
Where the rows and columns are numbered from `1` to `9`.

Each cell is one character:
- Hidden cells are `#`
- Revealed cells are a number `1` to `8`, or ` ` (space) (instead of `0`)
- Flagged cells are `F`
- If a mine cell were revealed, it would be `X` (e.g. for if player loses)

Sample user input (you may also mimic):
```
Enter row column (e.g. 1 2): 3 5
Enter action, r for reveal, f for flag: r
```
(The inputs are after the `: `, and on the same line.) \
The user enters the row column coordinate pair. Then, they choose the action. \
If the action is useless (e.g. revealing or flagging a dug cell), nothing happens. If the cell is flagged, then reveal action digs the cell ignoring the flag, while flag actions instead unflags the cell. \
If the action is invalid (e.g. out of bounds coordinates or action other than `r` or `f`) then the program tells the user it is invalid and nothing happens. (These validation checks are not required.) (Certain invalid inputs still cause the program to crash, such as non-numeric coordinates) \
The grid is displayed before each user turn, and is also displayed when the game ends.

<details><summary>Example single game input/output</summary>

```
9x9 grid, with 10 mines

   1 2 3 4 5 6 7 8 9 
  +-------------------+
1 | # # # # # # # # # |
2 | # # # # # # # # # |
3 | # # # # # # # # # |
4 | # # # # # # # # # |
5 | # # # # # # # # # |
6 | # # # # # # # # # |
7 | # # # # # # # # # |
8 | # # # # # # # # # |
9 | # # # # # # # # # |
  +-------------------+
Enter row column (e.g. 1 2): 5 5
Enter action, r for reveal, f for flag: r

    1 2 3 4 5 6 7 8 9
  +-------------------+
1 | # # # # # # # # # |
2 | # # # # 1 1 2 # # |
3 | # # # 1 1   1 # # |
4 | # # # 1     1 1 1 |
5 | # # # 1           |
6 | # # # 1           |
7 | # # # 2 2 1       |
8 | # # # # # 1       |
9 | # # # # # 1       |
  +-------------------+
Enter row column (e.g. 1 2): 2 4
Enter action, r for reveal, f for flag: f

    1 2 3 4 5 6 7 8 9
  +-------------------+
1 | # # # # # # # # # |
2 | # # # F 1 1 2 # # |
3 | # # # 1 1   1 # # |
4 | # # # 1     1 1 1 |
5 | # # # 1           |
6 | # # # 1           |
7 | # # # 2 2 1       |
8 | # # # # # 1       |
9 | # # # # # 1       |
  +-------------------+
Enter row column (e.g. 1 2): 8 4
Enter action, r for reveal, f for flag: r

    1 2 3 4 5 6 7 8 9
  +-------------------+
1 | # # # # # # # # # |
2 | # # # F 1 1 2 # # |
3 | # # # 1 1   1 # # |
4 | # # # 1     1 1 1 |
5 | # # # 1           |
6 | # # # 1           |
7 | # # # 2 2 1       |
8 | # # # X # 1       |
9 | # # # # # 1       |
  +-------------------+
You dug a mine!
Play again? (y/n): n
Exiting.
```
</details>

## Tips
- This is a difficult project that focuses on your problem solving skills. **Ask for help from mentors if you are struggling.**
- A complex program like this needs to be structured well.
    - Try to plan out what collections, classes, etc. you will use, before you start writing; consider the requirements and mechanics.
        - Not confident? Ask a mentor to critique your plans.
    - Be keen on defining methods, to abstract and simplify code.
        - If something is done multiple times throughout the code, extract it into a method
        - If one of your methods is too large, split into different parts and then define methods for each
- Try to get individual parts of the program working before moving on to the next. Even if your program is not completely functional, try to test implemented parts
- Nested for loops and indices will probably be used heavily, while OOP might not be used as much
- This course taught all of the topics needed to create this project, though you may research and use other topics as well

The purpose of this project is for mentors to assess your programming skills.

Again, get help from mentors when needed. We can:
- Review code early to make sure you are on the right track
- Help plan out the implementation and what will be needed
- Provide pointers on how to implement difficult features
- Give hints about how to fix elusive bugs
- Help test and identify bugs in your program
- Reteach old concepts or get additional support on new topics

___

## Feedback
Please provide overall feedback for both the final project and **the whole course**. \
Also, please give an estimate of how much time you spent on this project.

Possible prompts: \
Did the course meet your expectations? \
How was the pacing and the amount of content? \
Do you have any overall complaints about the teaching or structure? \
Was enough external support given? \
Describe how much effort, time, or commitment this course took. \
Rate your experience with this course.
___



## License
© 2025 @aatle, @spacepotatoes3, @gjgarson, @KaitoTLex

This work is licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).
