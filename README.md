<div align="center">

# 🎲 KiviBoardGame

A Java Swing adaptation of the dice strategy board game Kivi for 2–4 human or AI players.

![Java](https://img.shields.io/badge/Java-17+-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Java Swing](https://img.shields.io/badge/Java-Swing-2C2255?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-PC%20%7C%20macOS%20%7C%20Linux-6c757d?style=flat-square)
![Project](https://img.shields.io/badge/Project-Personal-blue?style=flat-square)
![Team](https://img.shields.io/badge/Team-Solo_Developer-success?style=flat-square)

[Demo Video](https://youtu.be/_-S2DVjqv3M?si=x7RK4brk-IgvsSm_) • [Download JAR](https://drive.google.com/file/d/1qtlXPWwPTnp1OaDCW8HUraQ7EOuCGrCU/view?usp=drive_link)

</div>

---

## Project Overview

KiviBoardGame delivers a full Java Swing adaptation of the dice strategy board game Kivi: 2–4 players, either human or AI, roll to match patterns on a 7×7 grid, build scoring lines over 10 rounds, and play with save/load support and color vision accessibility options.

- **Players:** 2–4
- **Studio:** Personal Project
- **Genre:** Dice Strategy Board Game
- **Platform:** PC, Mac OS, Linux
- **Engine & Tools:** IntelliJ, Java Swing
- **Duration:** 3 Months
- **Team Size:** Solo Developer

## Hero Screen

<p align="center">
  <img width="900" alt="KiviBoardGame hero screen" src="https://github.com/user-attachments/assets/e92bfd1e-fb68-435c-b610-f4e131b68741" />
</p>

## How to Run (JAR)

### Requirements

- Java installed (**JDK/JRE 17+ recommended**)

## Tech Stack

### Core Technical Stack
- Java
- Java Swing
- Object-Oriented Programming (OOP)
- GRASP Design Patterns
- Multi-threading
- Java Serialization

### Systems & Logic
- Game Architecture
- AI Heuristics & Logic
- Pattern Validation Algorithms
- State Management
- Concurrency Control

### UI/UX & Accessibility
- Event-Driven Programming
- Inclusive Design (CVD Support)
- Interface Decoupling
- Asset Integration

### Tools & Workflow
- GitHub
- IntelliJ IDEA
- Technical Documentation
- Figma

## Features & Contributions

### Setup & Main Menu

Configure 2–4 players with support for human or AI opponents, difficulty selection, player names, player colors, vision mode, and backgrounds through the `MainMenu`.

<p align="center">
  <img width="900" alt="Main menu setup animation" src="https://github.com/user-attachments/assets/f415cd1f-27a4-4837-82c8-5669c59f223c" />
</p>

### Core Gameplay Loop

Roll 6 dice up to 3 times, match the result to valid patterns on the grid, place a stone, update live player stats, and move to the next turn across a 10-round match.

<p align="center">
  <img width="900" alt="Dice pattern guide" src="https://github.com/user-attachments/assets/573f6aeb-d2f3-4367-9781-b7e17d15582c" />
</p>

<p align="center">
  <img width="700" alt="Active gameplay screen" src="https://github.com/user-attachments/assets/2d684b13-b136-4a2f-b899-91d40f145d68" />
</p>

### Player Stats & UI

Real-time player panels display names, colors, stones remaining, and scores, while ensuring only the active player can take actions.

<p align="center">
  <img width="280" alt="Player information panel" src="https://github.com/user-attachments/assets/a36373c7-962e-4b08-83ff-172882677fab" />
</p>

### Save / Load Feature

The game can serialize the full game state, including board, players, round data, and turn state, allowing matches to be paused and resumed later.

<p align="center">
  <img width="400" alt="Game save dialog" src="https://github.com/user-attachments/assets/39320a24-1380-4ed5-b951-7484c2c3538d" />
</p>

### Accessibility Options

Players can toggle a color vision deficiency mode with numbered grids and labeled stones, and also choose custom visual themes such as **Whimsy World** and **Critter Carnival**.

<p align="center">
  <img width="500" alt="Display settings menu" src="https://github.com/user-attachments/assets/31fdab79-0d27-4ad0-80bf-88ee10f5bad5" />
</p>

<p align="center">
  <img width="500" alt="Display settings confirmation" src="https://github.com/user-attachments/assets/5a3435cd-d7ae-4bc6-98ef-12cb5ab281fa" />
</p>

### Game Over & Win

At the end of the game, final scores are calculated and the winner is announced with a Duke mascot popup.

<p align="center">
  <img width="620" alt="Game over screen" src="https://github.com/user-attachments/assets/d8a75841-e0eb-4c51-9f26-491deb90a448" />
</p>

## Technical Implementation

### Architecture & GRASP

The game is structured around a central `GameManager` that acts as the controller. It coordinates turns, validates moves, updates the board and scores, and notifies the UI.

UI and logic stay decoupled through a `GridCellClickListener`, so grid clicks are handled separately from the game logic, supporting lower coupling. All dice-rule checks are centralized in a single `validateDiceCombination` method so rule changes do not ripple across the codebase, which supports protected variations.

The AI runs as a `ComputerPlayerAI` thread so the interface remains responsive during computer-controlled turns, which reflects the use of pure fabrication in the design.

### Dice Validation

A roll is valid if it matches one of three patterns:

- Sum over 30
- Three pairs
- A straight from 1–6

Validation is centralized: the dice are sorted first, then checked against each pattern. Only valid combinations allow the placement of a stone, and the grid highlights only cells that match the current roll.

### Real-Time Grid Highlighting

When the player rolls, the grid checks every empty cell and validates whether the current dice result satisfies that cell’s required pattern. Matching cells are highlighted visually, for example with a green border.

Transient UI state such as highlight markers is cleared and repainted on each update. Because the highlighting uses the same validation logic as placement, the interface always reflects the current roll accurately.

### AI Behavior

The **Easy AI** rolls at most twice and then selects a random valid placement.

The **Hard AI** gets up to three rolls and uses a `prioritizeBlocking` step to prefer moves that interrupt the opponent’s longest scoring line.

All AI logic runs on a background thread. Once the AI finishes its decision, it uses `SwingUtilities.invokeLater` to safely update the `GameManager` and end the turn on the Event Dispatch Thread.

### Persistence

Saving writes the full `GameManager` state, including board, players, round data, current player index, and overall game state, to an `ObjectOutputStream`.

Transient fields such as running threads are excluded from serialization. Loading reads the saved objects back and then calls `restoreUI` to rebuild panels and restore the state so the player can resume the match exactly where it left off.

## Why This Project Stands Out

- Recreates a full board game experience in **Java Swing**
- Supports both **human and AI players**
- Includes **save/load persistence** for long-running matches
- Implements **accessibility support** for color vision deficiency
- Uses **threading and EDT-safe updates** to keep the UI responsive
- Applies **GRASP principles** to separate game rules, UI interaction, and controller logic

## Suggested Project Structure

```text
.
├── src/                    # Source code
├── assets/                 # Images, backgrounds, and UI assets
├── saves/                  # Serialized save files
├── docs/                   # Technical documentation / design notes
└── README.md
```

## Demo and Download

- **Demo Video:** [Watch on YouTube](https://youtu.be/_-S2DVjqv3M?si=x7RK4brk-IgvsSm_)
- **Download JAR:** [Google Drive Link](https://drive.google.com/file/d/1qtlXPWwPTnp1OaDCW8HUraQ7EOuCGrCU/view?usp=drive_link)

## Future Improvements

- Add more AI difficulty levels
- Improve animations and feedback during dice rolls
- Expand accessibility options further
- Add a cleaner installer or packaged desktop release
- Add match history and replay support

## License

Add your preferred license here, or mark the repository as a personal portfolio project.
