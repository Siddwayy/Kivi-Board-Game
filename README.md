# KiviBoardGame

## Project Overview
KiviBoardGame delivers a full Java Swing adaptation of the dice strategy board game Kivi: 2–4 players (human/AI) roll to match patterns on a 7×7 grid, build scoring lines over 10 rounds, with save/load and color vision support.

- Players: 2–4  
- Studio: Personal Project  
- Genre: Dice Strategy Board Game  
- Platform: PC, Mac OS, Linux  
- Engine & Tools: IntelliJ, Java Swing  
- Duration: 3 Months  
- Team Size: Solo Developer  

Demo Video-https://youtu.be/_-S2DVjqv3M?si=x7RK4brk-IgvsSm_

Download Jar File-https://drive.google.com/file/d/1qtlXPWwPTnp1OaDCW8HUraQ7EOuCGrCU/view?usp=drive_link

<img width="755" height="421" alt="HeroScreen" src="https://github.com/user-attachments/assets/e92bfd1e-fb68-435c-b610-f4e131b68741" />

---

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


---

## Features & Contributions

### Setup & Main Menu
Configure 2–4 players: human/AI (Easy/Hard), names, colors, vision mode, and backgrounds via the MainMenu.

<img width="844" height="761" alt="MainMenu" src="https://github.com/user-attachments/assets/b6b292b8-18d1-47bf-9e0c-8b089e1d14e5" />


### Core Gameplay Loop
Roll 6 dice (up to 3 times) → grid highlights pattern matches → place stone → live stats update → next player. 10 rounds to win!

<img width="924" height="146" alt="DicePatterns" src="https://github.com/user-attachments/assets/ef28e770-b725-4877-afd1-d2962831d6fe" />
<img width="770" height="761" alt="ActiveGameplay" src="https://github.com/user-attachments/assets/2d684b13-b136-4a2f-b899-91d40f145d68" />



### Player Stats & UI
Real-time panels show names, colors (R/G/B/W), stones left, and scores – only the current player can act.

![PlayerInfo](https://github.com/user-attachments/assets/74a3e696-23ab-433d-8138-89fa28f43e78)


### Save/Load Feature
Serialize full game state (board, players, round) – pause and resume anytime.

<img width="407" height="246" alt="GameSave" src="https://github.com/user-attachments/assets/39320a24-1380-4ed5-b951-7484c2c3538d" />


### Accessibility Options
Toggle color vision deficiency mode (numbered grids 1–3, labeled stones) and custom backgrounds (Whimsy World, Critter Carnival).

<img width="492" height="239" alt="DisplaySettings" src="https://github.com/user-attachments/assets/31fdab79-0d27-4ad0-80bf-88ee10f5bad5" />
<img width="478" height="223" alt="DisplaySettingsConfirmation" src="https://github.com/user-attachments/assets/5a3435cd-d7ae-4bc6-98ef-12cb5ab281fa" />


### Game Over & Win
Final scores are calculated; winner is announced with a Duke mascot popup.

<img width="621" height="230" alt="GameOver" src="https://github.com/user-attachments/assets/d8a75841-e0eb-4c51-9f26-491deb90a448" />

---

## Technical Implementation

### Architecture & GRASP
The game is structured around a central GameManager that acts as the controller: it coordinates turns, validates moves, updates the board and scores, and notifies the UI. UI and logic stay decoupled via a GridCellClickListener so grid clicks are handled separately from game logic (low coupling). All dice-rule checks live in a single validateDiceCombination method so rule changes don’t ripple through the codebase (protected variations). The AI runs as a ComputerPlayerAI thread so the UI stays responsive during computer turns (pure fabrication).

### Dice validation
A roll is valid if it matches one of three patterns: sum over 30, three pairs, or a straight 1–6. Validation is centralized: dice are sorted, then checked against each pattern. Only valid combinations allow placing a stone, and the grid highlights only cells that match the current roll.

### Real-time grid highlighting
When the player rolls, the grid loops over all empty cells, checks whether the dice match each cell’s required pattern, and highlights valid cells (e.g. green border). Transient state like highlights is cleared and repainted each time. The logic is driven by the same validation used for placement, so the UI always reflects the current roll.

### AI behavior
Easy AI rolls at most twice and picks a random valid placement. Hard AI gets three rolls and uses a prioritizeBlocking step to prefer moves that block the opponent’s longest line. All AI work runs on a background thread; when done, it calls SwingUtilities.invokeLater to update the GameManager and end the turn on the EDT.

### Persistence
Save writes the full GameManager (board, players, round, state) plus current player index and game state to an ObjectOutputStream. Transient fields such as threads are not serialized. Load reads those objects back and then calls restoreUI to rebuild panels and state from the loaded model so the player can resume exactly where they left off.

<!-- INSERT SCREENSHOTS HERE -->
