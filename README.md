# Frogger (MATLAB)

A classic Frogger arcade game clone built entirely in MATLAB using a custom sprite-based game engine.

## About

Guide your frog across busy roads and a treacherous river to reach the safety of home. Dodge cars and trucks on the road, then hop onto floating logs to cross the river -- but watch out for crocodiles, snakes, and otters! Fill all five home spots to advance to the next level, where the game gets progressively faster and more dangerous.

## Features

- **5 progressive difficulty levels** with unique enemy layouts
- **Logarithmic speed curve** that increases challenge as you advance
- **Sound effects** for hopping, collisions, drowning, and reaching home
- **Background music** that loops during gameplay
- **Start menu** and **game-over screen** with click-to-restart
- **Score and time tracking** saved to `score_trophy.txt` after each game
- **Lives system** starting at 5 lives, with a bonus life earned per completed level
- **HUD** displaying current score, level, and remaining lives

## Requirements

- MATLAB (R2016b or later recommended)
- Image Processing Toolbox (for `imresize`, used by the game engine)
- A graphical desktop environment (the game renders in MATLAB figure windows)

## How to Play

1. Clone this repository:
   ```
   git clone https://github.com/sohumsuthar/frogger.git
   ```
2. Open MATLAB and set your working directory to the cloned repository folder.
3. Run the game:
   ```matlab
   frogger
   ```
4. Click the game window to start.

### Controls

| Key             | Action     |
|-----------------|------------|
| Up Arrow / W    | Move up    |
| Down Arrow / S  | Move down  |
| Left Arrow / A  | Move left  |
| Right Arrow / D | Move right |

### Gameplay

- **Road (rows 7-9):** Avoid cars, trucks, and other vehicles that scroll across the screen.
- **River (rows 2-5):** Jump onto logs and turtles to cross. Falling into the water costs a life.
- **Home (row 1):** Land on one of the five open home spots to score a point.
- Fill all 5 homes to advance to the next level and earn a bonus life.
- The game ends when all lives are lost. Your score and time are saved automatically.

## Project Structure

```
frogger.m                  Main entry point and game loop
initVars.m                 Game variable initialization
startScreen.m              Title screen display
keyPressCallback.m         Keyboard input handler
movementSpriteControl.m    Enemy and log animation
refreshScene.m             Scene rendering and HUD updates
gameEndCheck.m             Level completion and progression
xCheck.m                   Collision detection
resetFrogPos.m             Frog position reset and life/score management
restartGame.m              Game-over handling and restart logic
simpleGameEngine.m         Sprite-based rendering engine
letterIndex.m              Character-to-sprite mapping
numPref.m                  Digit-to-sprite mapping
tensDigit.m                Tens digit extraction utility
onesDigit.m                Ones digit extraction utility
frogger.png                Sprite sheet (16x16 tiles)
frogger.psd                Sprite sheet source (Photoshop)
kensample.mp3              Background music
sound-frogger-*.wav        Sound effects
```

## License

This project is licensed under the GNU General Public License v3.0. See [LICENSE](LICENSE) for details.
