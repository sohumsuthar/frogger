# CLAUDE.md

## Project Overview

This is a **Frogger** clone written entirely in MATLAB, built as a course project (ENGR 1181). The player controls a frog that must cross a road with moving vehicles and a river with floating logs to reach home spots at the top of the screen. The game features multiple levels with progressive difficulty, sound effects, background music, a start menu, a game-over screen, score tracking, and a lives system.

## Tech Stack

- **Language:** MATLAB (`.m` files)
- **Game Engine:** `simpleGameEngine` -- a custom MATLAB class (provided as course infrastructure) that renders sprite-based tile grids using MATLAB's figure/image system
- **Graphics:** 16x16 pixel sprites loaded from `frogger.png` sprite sheet, rendered at 5x zoom
- **Audio:** WAV files for sound effects, MP3 for background music, played via MATLAB's `audioplayer` and `sound`
- **License:** GPL-3.0

## Directory Structure

```
frogger/
  frogger.m                  -- Main entry point / game loop
  frogger_allfuncs.m         -- Single-file version with all functions inlined (for MATLAB publishing)
  initVars.m                 -- Initializes all global game variables, layers, and the game engine
  startScreen.m              -- Displays the start menu and waits for mouse click
  keyPressCallback.m         -- Handles keyboard input (arrow keys / WASD) for frog movement
  movementSpriteControl.m    -- Animates moving sprites (cars, logs, enemies) via circular shift
  refreshScene.m             -- Redraws the scene, updates HUD (score/lives/level), handles game-over display
  gameEndCheck.m             -- Checks if frog reached a home; handles level progression (levels 1-5)
  xCheck.m                   -- Checks if the frog collided with an enemy or fell in water
  resetFrogPos.m             -- Resets frog to starting position; decrements lives or increments points
  restartGame.m              -- Saves score to file, waits for click, re-initializes for a new game
  letterIndex.m              -- Maps characters ('s','t','a','r','g','a','m','e','o','v','r') to sprite indices
  numPref.m                  -- Maps digit values (0-9) to sprite indices for score/lives display
  tensDigit.m                -- Extracts the tens digit from a number
  onesDigit.m                -- Extracts the ones digit from a number
  simpleGameEngine.m         -- Sprite-based game engine class (handles rendering, keyboard, mouse input)
  frogger.png                -- Main sprite sheet (16x16 tiles)
  gameSprites.png            -- Additional sprite sheet (unused in code)
  sprites.png                -- Additional sprite sheet (unused in code)
  another sprite sheet.png   -- Additional sprite sheet (unused in code)
  frogger.psd                -- Photoshop source for the sprite sheet
  kensample.mp3              -- Background music
  sound-frogger-hop.wav      -- Hop sound effect (currently commented out)
  sound-frogger-extra.wav    -- Sound played when frog reaches a home
  sound-frogger-squash.wav   -- Sound played when frog is hit by an enemy
  sound-frogger-plunk.wav    -- Sound played when frog falls in water
  score_trophy.txt           -- Persisted score and time from last game session
  html/                      -- MATLAB-published HTML versions of the scripts
  *.asv                      -- MATLAB auto-save backup files (can be ignored)
  LICENSE                    -- GPL-3.0 license
  .gitattributes             -- Git line-ending normalization config
```

## How to Run

1. Open MATLAB (any recent version with the Image Processing Toolbox for `imresize`).
2. Set the working directory to the repository root (where `frogger.m` is located).
3. Run `frogger` in the MATLAB Command Window.
4. Click the game window when the start screen appears to begin playing.

## Controls

- **Arrow keys** or **WASD** -- Move the frog (up/down/left/right)
- **L key** -- Add an extra life (debug cheat)
- **Mouse click** -- Start the game from the title screen or restart after game over

## Architecture and Key Concepts

### Rendering Layers

The game uses a three-layer rendering system via `simpleGameEngine.drawScene()`:

1. **bottomL** (bottom layer) -- Static background: water tiles, grass/sidewalk tiles, home row bushes
2. **topL** (top layer) -- Moving objects: logs, turtles, cars, trucks, crocodiles, snakes, otters
3. **blankL** (blank/frog layer) -- The frog sprite, HUD text (score/lives/level), and game-over text

### Game Loop

The main loop in `frogger.m`:
1. Calls `movementSpriteControl()` to circularly shift enemy/log rows each tick
2. Calls `refreshScene()` to redraw the scene
3. Pauses for `speed` seconds (logarithmic difficulty curve)
4. Keyboard input is handled asynchronously via `keyPressCallback` (figure KeyPressFcn)

### Level Progression

- 5 home slots must be filled per level
- Levels 1-5 each define a unique `topL` layout with progressively more enemies
- Speed increases logarithmically: `speed = (-0.2 * log((level+1)/3) + 0.8) * multiplier`
- Player gains 1 extra life per completed level

### Global State

All game state is managed through MATLAB `global` variables. Key globals:
- `scn` -- simpleGameEngine scene object
- `frogPos` -- [row, col] position of the frog
- `topL`, `bottomL`, `blankL` -- 12x11 matrices of sprite indices
- `points`, `lives`, `level`, `speed`, `gameOver`
- `enemies` -- array of sprite indices that kill the frog on contact
- `movementRows` -- which rows animate (shift) each tick
- `homes` -- 1x5 array tracking which home slots are filled

## Coding Conventions

- All functions are in separate `.m` files (one function per file, matching the filename)
- `frogger_allfuncs.m` is a monolithic copy with all functions as local functions (for MATLAB Publish)
- Heavy use of MATLAB `global` variables for shared state
- Comments are inline and descriptive
- Sprite indices are hardcoded integers referencing positions in the sprite sheet
- `.asv` files are MATLAB auto-save backups and should not be edited directly

## Important Notes

- The game requires a graphical MATLAB desktop environment (it opens figure windows).
- The `simpleGameEngine` class requires `imresize` from the Image Processing Toolbox.
- The `frogger_allfuncs.m` file is a convenience copy; the canonical code is the set of individual `.m` files.
- Sound effects for hopping (`sound-frogger-hop.wav`) are loaded but currently commented out in `keyPressCallback.m`.
- The `score_trophy.txt` file is overwritten each time the game ends.
- The grid is 12 rows by 11 columns. Row 1 is the home row, rows 2-5 are the river, row 6 is a median, rows 7-9 are the road, row 10 is a sidewalk, row 11 is the frog start row, and row 12 is the HUD.
