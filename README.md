# Tetris for Android

A small Tetris clone for Android, written in Java in March 2018 as a short
assignment ("Tarea Corta 2") for a mobile development course in college.

It's kept here as it was submitted: a snapshot of early work, not a maintained
project.

## Gameplay

- A random piece from the seven standard tetrominoes (I, O, T, S, Z, J, L)
  spawns at the top and falls one row every 400 ms.
- On-screen buttons move the piece **left** and **right** and **rotate** it.
- When the bottom row is full it is cleared and counts as one line.
- The game ends when a new piece can't move down.
- **Salir** (Exit) stops the game and shows how many lines you cleared.

## How it works

The code is three classes in
[`app/src/main/java/com/example/josue/tetris2`](app/src/main/java/com/example/josue/tetris2):

| Class | Role |
| --- | --- |
| `Tetris` | The activity. It runs the game loop on a `java.util.Timer`, keeps a 14×13 `float` grid of the board (the outer cells are walls), and handles button presses. |
| `Shape` | A tetromino made of four `Square`s. It spawns one of the seven piece types and holds a hand-written rotation table for each type's four orientations. |
| `Square` | Wraps one `ImageView` block with an id. |

Blocks aren't drawn on a canvas. The layout declares a fixed pool of 121
`ImageView`s that start out invisible. Each new piece takes the next four from
the pool, makes them visible and moves them by setting their X/Y position.
Before each move, the board position is worked out from those pixel
coordinates.

## Known limitations

This was written against a deadline, and it shows:

- **Line clearing is simplified.** Only the bottom row is checked, only one
  line clears per piece, and clearing shifts *every* block down, not just the
  blocks above the cleared row.
- **Fixed pixel sizes.** Cells are hardcoded as 70 px and the board as
  400×455 dp, so the layout only lines up on screens similar to the one it was
  tested on.
- **The block pool is finite.** After 120 blocks (30 pieces) the pool wraps
  around and reuses views that may still be on the board.
- **Threading.** The game loop moves views from a background `Timer` thread
  instead of the UI thread.
- **Rotation isn't collision-checked,** so pieces can rotate into walls or
  other blocks.

## Building

The project uses the 2018 toolchain: Android Gradle Plugin 3.0.1, Gradle 4.1,
the `android.support` libraries and `compileSdkVersion 26`. It also pulls
dependencies from `jcenter()`, which has since been shut down. As a result it
won't build as-is in a current Android Studio without upgrading the build
files and migrating to AndroidX.

To look at the code, just browse the sources above. To try building it,
open the project in Android Studio and follow the upgrade assistant's
prompts.
