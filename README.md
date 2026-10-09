# Harvest Match 🐱🍎

**A Java-based Match-3 puzzle game featuring custom board layouts, cascading matches, special pieces, and persistent game state.**

[Gameplay Demo 1](https://github.com/user-attachments/assets/a715c730-2ddf-40f5-a798-38ac3bfeda17
) · [Gameplay Demo 2](https://github.com/user-attachments/assets/060d81ab-83ac-4778-8bdc-9c515a199661) · [Original Repository](https://github.com/chromics/CandyCrush)

![Harvest Match Home Screen](https://github.com/user-attachments/assets/31ae61e1-7f14-4bcb-83e2-528651769374)

## Overview

Harvest Match is a Match-3 puzzle game developed by a two-person team using Java and JavaFX. Players swap pieces to form matches, trigger cascading clears, and complete objectives across different board layouts and game modes.

My contribution focused on the **Java game logic, data models, algorithms, and save/load functionality**. My teammate developed the JavaFX interface, visual animations, and audio system.

### Key Features

* **Dynamic board layouts:** Rectangular boards and irregular shapes, including octagons, T-shapes, and Y-shapes.
* **Special pieces:** Match four or more pieces to create horizontal- or vertical-clearing special pieces.
* **Obstacle mechanics:** Destroy ice blocks to complete special-level objectives.
* **Board generation:** Prevent initial matches and ensure that generated boards contain at least one valid move.
* **Hint system:** Identify potential matches using spatial pattern recognition.
* **Cascading gameplay:** Remove matched pieces, apply gravity, generate replacements, and resolve subsequent matches.
* **Save/load:** Preserve game progress, including the board state, score, and remaining moves.
* **Custom levels:** Configure board dimensions, map templates, move limits, shuffle limits, and target scores.

## Technology Stack

| Area                 | Technologies                                        |
| -------------------- | --------------------------------------------------- |
| Language             | Java 17+                                            |
| User interface       | JavaFX, FXML — teammate's contribution              |
| Programming concepts | Object-oriented design, algorithms, data structures |
| State persistence    | File I/O, text parsing, Java object serialization   |
| Architecture         | Model, game mechanisms, controllers, and views      |

## My Contributions

I implemented the core Java game engine and its associated data-handling components.

### 1. Game Models and State Management

* Implemented the core data models, including `Board`, `Cell`, `BoardPoint`, and `Piece`, to represent the game grid, coordinates, and pieces.
* Developed `GameData` to maintain gameplay state, including scores, remaining moves, matches, and level objectives.
* Supported irregular board layouts through `MapTemplate`, which marks non-playable cells while preserving a common board representation.

### 2. Match Detection and Cascading Resolution

* Implemented horizontal and vertical match detection in `CheckMatch`, identifying sequences of three or more matching pieces.
* Developed `Fall` to move existing pieces into empty cells and generate replacements after matches are removed.
* Integrated the game-resolution sequence in `GameController`, coordinating swaps, match detection, piece removal, falling pieces, subsequent cascades, and win/loss checks.

### 3. Board Generation and Hint Finding

* Implemented `CreateBoard` to generate playable boards while avoiding unintended matches during initialization.
* Used predefined valid-move patterns to ensure that newly generated boards have at least one potential move.
* Developed `HintFinder` to identify candidate moves by checking spatial patterns of adjacent pieces rather than exhaustively simulating every possible swap.

### 4. Save/Load and Persistence

* Implemented a text-based save/load mechanism in `SaveLoadController`, parsing and writing game state to local `.txt` files.
* Supported persistence of board dimensions, piece positions, ice blocks, scores, and other gameplay state required to resume a game.
* Implemented an alternative serialization-based approach through `SaveLoadControllerSer`, using Java object serialization and `.ser` files.

### 5. Special Gameplay Mechanics

* Implemented special-piece behavior for clearing pieces horizontally or vertically.
* Integrated ice-block removal into the match-resolution flow for special game modes.
* Supported configurable board layouts and gameplay parameters for custom levels.

## Technical Design

This section describes the key implementation decisions behind the game engine.

### Board Generation

Random initialization can produce unwanted matches or boards without valid moves. `CreateBoard` addresses these issues through two steps:

1. **Ensure a valid move:** Place a predefined potential-match pattern into suitable playable cells.
2. **Avoid initial matches:** When filling remaining cells, check the preceding horizontal and vertical pieces and exclude piece types that would immediately form a match of three.

This produces boards that are playable from the beginning without requiring an initial match-clearing sequence.

### Match Detection

`CheckMatch` scans the board horizontally and vertically, tracking the start of each sequence of identical pieces.

When the piece type changes or a boundary is reached, the algorithm evaluates the sequence length. Sequences of at least three pieces are recorded as `MatchData`, including their coordinates, length, and orientation.

### Gravity and Cascading Matches

After matched pieces are removed, `Fall` compacts the remaining pieces within each column and fills newly available positions with random pieces.

The process is coordinated with the game-resolution loop so that newly formed matches can be detected and resolved repeatedly until the board stabilizes.

### Irregular Board Layouts

`MapTemplate` defines the supported board shapes. Each template identifies cells that should be marked `NOTPLAYABLE`, allowing the same underlying board representation to support different geometries.

This keeps board-shape configuration separate from the core match-detection and falling mechanisms.

### Hint Finding

`HintFinder` searches for spatial patterns that could become matches after a single swap. It checks relative cell positions against predefined almost-match patterns and returns candidate coordinates for the interface to highlight.

### Game State and Persistence

The game state is represented by `GameData`, which stores the information needed to continue a session.

Two persistence approaches are supported:

* **Text-based persistence:** Explicitly writes and parses game-state information in `.txt` files.
* **Object serialization:** Uses Java serialization to save and restore serializable game objects in `.ser` files.

## Team Contributions

This was a two-person project with a clear division of responsibilities.

| Component                                            | Responsibility          |
| ---------------------------------------------------- | ----------------------- |
| Game models and data structures                      | My contribution         |
| Match detection, board generation, and falling logic | My contribution         |
| Hint finding and game-rule orchestration             | My contribution         |
| Save/load and state persistence                      | My contribution         |
| JavaFX scenes and FXML layouts                       | Teammate's contribution |
| UI event handling and visual rendering               | Teammate's contribution |
| Piece-swap and falling animations                    | Teammate's contribution |
| Music, sound effects, sprites, and visual assets     | Teammate's contribution |

## Gameplay Demos

### Main Menu

<!-- TODO: Add a screenshot of the main menu at assets/home-screen.png -->

![Harvest Match Main Menu](https://github.com/user-attachments/assets/31ae61e1-7f14-4bcb-83e2-528651769374)

### Gameplay Demo 1 — Normal Mode

<!-- TODO: Place the first gameplay video at demos/gameplay-normal.mp4 -->

[Watch Gameplay Demo 1](https://github.com/user-attachments/assets/a715c730-2ddf-40f5-a798-38ac3bfeda17)

### Gameplay Demo 2 — Special Mode

<!-- TODO: Place the second gameplay video at demos/gameplay-special.mp4 -->

[Watch Gameplay Demo 2](https://github.com/user-attachments/assets/060d81ab-83ac-4778-8bdc-9c515a199661)

## How to Run

The project requires Java 17 or later and a compatible JavaFX SDK.

1. Clone or download the repository.
2. Open the project in an IDE that supports Java and JavaFX.
3. Configure the JavaFX SDK and ensure the required JavaFX modules are available.
4. Set the source directory and project dependencies according to the existing project structure.
5. Run `view.Main`.

If launching from the command line, configure the JavaFX module path and required modules, such as `javafx.controls` and `javafx.fxml`, according to your installed JavaFX version.

Exact build commands may depend on the repository's build configuration.

## Acknowledgements

Harvest Match was developed collaboratively. I implemented the Java game engine, data structures, algorithms, and persistence mechanisms, while my teammate implemented the JavaFX user interface, animations, and audio system.

See the [original repository](https://github.com/chromics/CandyCrush) for the original team project.
