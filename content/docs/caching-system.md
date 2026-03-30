---
title: "Caching System"
date: 2026-03-30T00:00:00+11:00
description: "How the TTL cache works and when to use it for computed game data"
category: "software"
author: "TRP Team"
robot: "v2"
version: "1.0"
subsystem: "core"
draft: false
---

## Overview

The caching system is a simple key-value store with automatic expiry. It exists because we were doing the same calculations over and over every tick — ball velocity, nearest robot, intercept points — and it was wasting cycles.

Now you compute it once, cache it, and anyone who needs it grabs it from the cache instead of recalculating.

## Basic Usage

```python
from teamcontrol.core.cache import Cache

cache = Cache(ttl=2.0)

# store something
cache.put("ball_velocity", (vx, vy))

# get it back
vel = cache.get("ball_velocity")  # None if expired or missing
```

## TTL (Time to Live)

Every cached value has a TTL — how long it stays valid. After that, it's automatically removed on the next access. You can set a default TTL when creating the cache, or override it per entry:

```python
cache = Cache(ttl=5.0)  # default: 5 seconds

cache.put("field_geometry", geo)               # uses the 5s default
cache.put("ball_velocity", vel, ttl=0.5)       # only valid for 0.5s
cache.put("team_config", cfg, ttl=60.0)        # valid for a full minute
```

Pick your TTL based on how fast the data changes:

| Data Type | Suggested TTL |
|-----------|--------------|
| Ball velocity | 0.1 - 0.5s |
| Robot positions | 0.5 - 1.0s |
| Nearest robot calculations | 1.0 - 2.0s |
| Field geometry | 30 - 60s |
| Config values | 60s+ |

## Namespaces

Use dot-separated keys to group related data. Then you can fetch or invalidate a whole group at once:

```python
# store per-robot data
cache.put("robot.0.position", (x0, y0))
cache.put("robot.0.velocity", (vx0, vy0))
cache.put("robot.1.position", (x1, y1))
cache.put("robot.1.velocity", (vx1, vy1))

# grab all robot data
all_robots = cache.get_ns("robot")
# returns: {"robot.0.position": ..., "robot.0.velocity": ..., ...}

# team switched — clear all robot cache
cache.invalidate_ns("robot")
```

## Size Limits

The cache has a max size (default 1024 entries). When it's full, the oldest entry gets evicted. You can change this:

```python
cache = Cache(ttl=5.0, max_size=256)
```

## Stats

Check how well the cache is performing:

```python
print(cache.stats)
# {'size': 42, 'hits': 1337, 'misses': 28, 'hit_rate': '97.9%'}
```

## World Model Cache

The WorldModel has a built-in cache for computed values:

```python
# inside a robot process
wm.cache.put("computed.nearest_opponent", nearest_id, ttl=1.0)
nearest = wm.cache.get("computed.nearest_opponent")
```

When a new frame is added to the world model, all keys under the `computed` namespace are automatically invalidated so you don't accidentally use stale calculations.

## Thread Safety

The cache is thread-safe (uses a lock internally). It's designed for single-process use though. For cross-process caching, use the WorldModel's cache through the manager proxy.

## When NOT to Use the Cache

- **Raw vision data** — that goes into the frame list, not the cache
- **Game controller state** — stored directly on the WorldModel
- **Config values** — those are in the Config object
- **Anything that needs to persist across restarts** — the cache is in-memory only
