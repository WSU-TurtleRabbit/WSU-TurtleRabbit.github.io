---
title: "Getting Started with 2026 Team Control"
date: 2026-03-30T00:00:00+11:00
description: "How to set up and run the 2026 Team Control system from scratch"
category: "general"
author: "TRP Team"
robot: "v2"
version: "1.0"
draft: false
---

## Overview

This guide walks you through setting up the 2026 Team Control system on your machine. If you've used the old `trp-1.0.0-test` codebase before, this will feel familiar but cleaner.

## Prerequisites

- Python 3.11 or newer
- pip (comes with Python)
- grSim installed (for simulation) — see the scripts folder for install helpers

## Installation

### Step 1: Clone the repo

```bash
git clone <repo-url>
cd 2026-TeamControl
```

### Step 2: Install dependencies

For basic development (no UI or simulation):
```bash
pip install -e .
```

If you want everything:
```bash
pip install -e ".[all]"
```

Or pick what you need:
```bash
pip install -e ".[sim]"       # adds pygame for simulation
pip install -e ".[ui]"        # adds PySide6 for the dashboard
pip install -e ".[planning]"  # adds Shapely, networkx, sklearn
pip install -e ".[dev]"       # adds pytest and ruff
```

### Step 3: Configuration

The system ships with sensible defaults in `src/teamcontrol/config/defaults.yaml`. If you need to change anything (like network addresses), create a `config.yaml` in the project root:

```yaml
team:
  us_yellow: false

vision:
  use_grsim: true

grSim:
  ip: "127.0.0.1"
  port: 20011
```

Only include the values you want to override — everything else falls back to the defaults.

### Step 4: Run it

```bash
python main.py
```

With a custom config:
```bash
python main.py --config config.yaml
```

## Running Tests

```bash
pytest
```

## What's Working Right Now

The base infrastructure is in place:

- **Config system** — YAML-based with defaults + overrides
- **World Model** — shared state across processes via multiprocessing Manager
- **Cache** — TTL-based in-memory cache with namespace support
- **Worker base class** — for building new background processes
- **Event bus** — lightweight pub/sub within a single process
- **Logger** — per-process log files with timestamps

## What's Coming Next

- Vision receiver (SSL-Vision / grSim)
- Game controller integration
- Dispatcher (sending commands to robots)
- Robot behaviour (goalie, striker, etc.)
- Path planning
- UI dashboard
