# Asteroids

A classic Asteroids clone built with [Pygame](https://www.pygame.org/), based on the [boot.dev](https://boot.dev) Python guided project. Pilot a ship, blast asteroids, and dodge debris as bigger rocks split into smaller ones on impact.

## Gameplay

- Fly your ship around an open-space, black-background arena
- Shoot asteroids to break them apart, each hit splits a large asteroid into two smaller, faster-moving fragments, until they're small enough to be destroyed outright
- Colliding with an asteroid ends the game
- New asteroids continuously spawn from the edges of the screen over time

## Controls

| Key | Action |
|---|---|
| `W` | Move forward |
| `S` | Move backward |
| `A` | Rotate left |
| `D` | Rotate right |
| `Space` | Shoot |

## Requirements

- Python 3.13+
- [pygame](https://www.pygame.org/) 2.6.1

## Installation & running

This project uses [uv](https://docs.astral.sh/uv/) for dependency management (see `pyproject.toml` / `uv.lock`).

```bash
# clone the repo
git clone https://github.com/tiagomxndes/pygame-asteroids.git
cd pygame-asteroids

# install dependencies with uv
uv sync

# run the game
uv run main.py
```

Alternatively, with plain pip:

```bash
python -m venv venv
source venv/bin/activate  # on Windows: venv\Scripts\activate
pip install pygame==2.6.1
python main.py
```

## Project structure

| File | Responsibility |
|---|---|
| `main.py` | Game loop — event handling, updating/drawing sprite groups, collision checks |
| `constants.py` | All tunable game constants (screen size, speeds, radii, cooldowns) |
| `circleshape.py` | Base class for circular game objects, providing shared position/velocity/collision logic |
| `player.py` | Player ship — movement, rotation, shooting, and rendering as a triangle |
| `asteroid.py` | Asteroid behavior, movement, and the split-on-hit mechanic |
| `asteroidfield.py` | Spawns new asteroids at random edges of the screen over time |
| `shot.py` | Player projectiles |
| `logger.py` | Lightweight event/state logging (e.g. asteroid splits, player hits) |

## How it works

The game uses Pygame's [`sprite.Group`](https://www.pygame.org/docs/ref/sprite.html) system to organize game objects into four groups:

- **`updatable`** — objects that need `update(dt)` called each frame (player, asteroids, shots, the asteroid field spawner)
- **`drawable`** — objects that get rendered to the screen each frame
- **`asteroids`** — used specifically for collision checks against the player and shots
- **`shots`** — used specifically for collision checks against asteroids

Each class declares which groups it belongs to via a `containers` class attribute, so simply instantiating a `Player`, `Asteroid`, or `Shot` automatically registers it in the right groups, no manual list management needed in the main loop.

Movement and shooting use `pygame.Vector2` rotation to convert the ship's current facing angle into a direction vector, scaled by speed and delta time (`dt`) for frame-rate-independent motion.

## Credits

Built as part of the [boot.dev](https://boot.dev) "Build a video game in Python" project, extended with custom tweaks.
