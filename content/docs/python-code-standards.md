---
title: "Python Code Cleanliness & Standards"
date: 2026-03-30T00:00:00+11:00
description: "Code style guide and cleanliness standards for all Python code in the TurtleRabbit project"
category: "software"
author: "TRP Team"
version: "1.0"
draft: false
---

## Overview

This document defines the coding standards and cleanliness expectations for all Python code in the TurtleRabbit project. Following these guidelines keeps our codebase consistent, readable, and maintainable across contributors.

## Style Guide

We follow [PEP 8](https://peps.python.org/pep-0008/) as our baseline. The key rules are summarised below.

### Naming Conventions

| Type | Convention | Example |
|------|-----------|---------|
| Variables | `snake_case` | `ball_position` |
| Functions | `snake_case` | `calculate_trajectory()` |
| Classes | `PascalCase` | `RobotController` |
| Constants | `UPPER_SNAKE_CASE` | `MAX_SPEED` |
| Private members | Leading underscore | `_internal_state` |
| Modules/files | `snake_case` | `motion_planner.py` |

### Indentation & Formatting

- Use **4 spaces** per indent level (no tabs).
- Maximum line length: **88 characters** (Black formatter default).
- Use blank lines to separate logical sections:
  - 2 blank lines before and after top-level definitions (classes, functions).
  - 1 blank line between methods inside a class.

### Imports

Organise imports in three groups, separated by a blank line:

```python
# 1. Standard library
import os
import math

# 2. Third-party packages
import numpy as np

# 3. Local project imports
from strategy.roles import Attacker
```

- Avoid wildcard imports (`from module import *`).
- Use absolute imports over relative imports when possible.

## Code Cleanliness

### Functions

- Keep functions short and focused — each function should do **one thing**.
- Limit function arguments to 4 or fewer. If you need more, group related parameters into a dataclass or dictionary.
- Use descriptive names — `get_closest_robot()` not `gcr()`.

```python
# Good
def get_closest_robot(position: tuple, robots: list) -> Robot:
    """Return the robot nearest to the given position."""
    return min(robots, key=lambda r: distance(position, r.position))

# Bad
def gcr(p, r):
    return min(r, key=lambda x: distance(p, x.position))
```

### Type Hints

Use type hints on all function signatures. This improves readability and enables static analysis.

```python
def move_to_target(robot_id: int, target: tuple[float, float], speed: float = 1.0) -> bool:
    """Command a robot to move to the target position."""
    ...
```

### Docstrings

Use docstrings for all public functions, classes, and modules. Follow the Google style:

```python
def kick_ball(power: float, angle: float) -> bool:
    """Kick the ball at the given power and angle.

    Args:
        power: Kick strength from 0.0 to 1.0.
        angle: Direction in radians relative to the robot's heading.

    Returns:
        True if the kick command was sent successfully.
    """
    ...
```

### Comments

- Write comments to explain **why**, not **what**.
- Remove commented-out code — use git history instead.
- Keep comments up to date when you change the code.

```python
# Good — explains intent
# Clamp speed to avoid burning out the motors on the v2 drivetrain
speed = min(speed, MAX_SAFE_SPEED)

# Bad — restates the code
# Set speed to the minimum of speed and MAX_SAFE_SPEED
speed = min(speed, MAX_SAFE_SPEED)
```

## Project Structure

Keep Python files organised by concern:

```
src/
├── strategy/       # Game strategy and roles
├── motion/         # Path planning and movement
├── vision/         # Vision processing
├── comms/          # Communication with robots
├── utils/          # Shared utility functions
└── tests/          # Unit and integration tests
```

- One class per file when the class is substantial.
- Group small, related helper functions into a shared module.

## Error Handling

- Catch specific exceptions, never bare `except:`.
- Use logging instead of `print()` for runtime messages.

```python
# Good
try:
    connection.send(command)
except ConnectionError:
    logger.error("Failed to send command to robot %d", robot_id)

# Bad
try:
    connection.send(command)
except:
    print("something went wrong")
```

## Tooling

We recommend the following tools to enforce these standards automatically:

| Tool | Purpose | Command |
|------|---------|---------|
| **Black** | Code formatter | `black .` |
| **isort** | Import sorter | `isort .` |
| **flake8** | Linter | `flake8 .` |
| **mypy** | Type checker | `mypy .` |

Run these before committing:

```bash
black . && isort . && flake8 . && mypy .
```

## Quick Checklist

Before submitting a pull request, verify:

- [ ] Code follows `snake_case` / `PascalCase` naming conventions
- [ ] All functions have type hints and docstrings
- [ ] No commented-out code or `print()` debug statements
- [ ] Imports are organised (stdlib → third-party → local)
- [ ] Black and flake8 pass with no errors
- [ ] Functions are focused and under ~30 lines each
- [ ] Specific exceptions are caught (no bare `except`)

## Related Documents

- [Contributing to TurtleRabbit Documentation](../contributing)
- [Project Structure](../project-structure)
