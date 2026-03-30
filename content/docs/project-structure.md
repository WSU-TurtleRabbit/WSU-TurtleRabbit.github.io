---
title: "Project Structure"
date: 2026-03-30T00:00:00+11:00
description: "How the 2026 Team Control codebase is organised and where to find things"
category: "software"
author: "TRP Team"
robot: "v2"
version: "1.0"
subsystem: "architecture"
draft: false
---

## Overview

The 2026 codebase is structured to keep things modular. Each subsystem gets its own package so you can work on one part without tripping over everything else.

## Directory Layout

```
2026-TeamControl/
├── main.py                  # entry point — starts everything
├── pyproject.toml           # dependencies and build config
├── config.yaml              # your local overrides (not committed)
│
├── src/teamcontrol/         # all source code lives here
│   ├── config/              # configuration loading
│   │   ├── settings.py      # Config class
│   │   └── defaults.yaml    # default values
│   │
│   ├── core/                # shared infrastructure
│   │   ├── cache.py         # TTL cache system
│   │   ├── worker.py        # base class for background processes
│   │   └── events.py        # event bus (pub/sub)
│   │
│   ├── world/               # game state
│   │   ├── model.py         # WorldModel — the single source of truth
│   │   └── manager.py       # multiprocessing manager wrapper
│   │
│   ├── robot/               # robot behaviour and constants
│   │   └── constants.py     # field geometry, speeds, thresholds
│   │
│   ├── network/             # UDP sockets, protobuf, comms (TODO)
│   ├── ssl/                 # SSL-Vision, game controller (TODO)
│   ├── strategy/            # behaviour trees, formations (TODO)
│   │
│   └── utils/               # shared utilities
│       ├── logger.py        # per-process file logging
│       └── maths.py         # common math helpers
│
├── tests/                   # pytest tests
├── content/docs/            # documentation (you are here)
├── scripts/                 # install scripts for grSim, etc.
├── logs/                    # log files (gitignored)
└── static/images/docs/      # images for documentation
```

## Key Design Decisions

### Why multiprocessing instead of async?

The old codebase used multiprocessing and it worked well for us. Vision, game controller, and dispatch each run as their own process. The WorldModel is shared via a Manager proxy. This keeps things isolated — if one process crashes, the others keep going.

### Why a separate cache?

We were recalculating stuff like ball velocity and robot distances every tick across multiple processes. The cache lets us compute something once and reuse it until it expires. The TTL ensures we never use stale data.

### Package naming

We switched from `TeamControl` (capital T, capital C) to `teamcontrol` (all lowercase). Python convention, easier to type, less room for import errors.

## Adding a New Module

1. Create a new directory under `src/teamcontrol/`
2. Add an `__init__.py`
3. Write your code
4. Import it where needed

If your module needs to run as a background process, extend `BaseWorker` from `teamcontrol.core.worker`.
