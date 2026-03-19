---
title: "Software Architecture"
date: 2025-01-20T00:00:00+11:00
description: "Overview of the TurtleRabbit software stack including strategy, vision, and communication systems."
category: "software"
author: "TurtleRabbit Team"
version: "1.0"
---

## Overview

The TurtleRabbit software system is responsible for making strategic decisions, processing vision data, and sending commands to the robots on the field. This document outlines the high-level architecture.

## System Components

### SSL-Vision

SSL-Vision is a shared vision system used by all SSL teams. Cameras mounted above the field detect robot and ball positions, providing this data to both teams over the network.

### Strategy & AI

The strategy layer decides what each robot should do based on the current game state. This includes:

- **Game state management** - Responding to referee commands
- **Role assignment** - Deciding which robot plays which role
- **Tactical decisions** - Attacking, defending, set plays

### Motion Planning

Converts high-level commands (e.g., "go to position X") into trajectories that avoid obstacles and respect the robot's physical constraints.

### Communication

Commands are sent wirelessly from the strategy computer to the robots on the field. Each robot receives velocity commands and kicker/dribbler instructions.

## Data Flow

```
SSL-Vision Cameras
       |
       v
  Vision Server (shared)
       |
       v
  TurtleRabbit Strategy
       |
       v
  Motion Planning
       |
       v
  Wireless Communication
       |
       v
  Robot Firmware
```

## Related Documents

- [Robot Overview](/docs/robot-overview/)
- [How to Contribute Documentation](/docs/contributing-to-docs/)
