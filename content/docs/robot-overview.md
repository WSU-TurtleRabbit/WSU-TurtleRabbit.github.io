---
title: "Robot Overview"
date: 2025-01-15T00:00:00+11:00
description: "High-level overview of the TurtleRabbit SSL robot design, key subsystems, and design philosophy."
category: "mechanical"
author: "TurtleRabbit Team"
robot: "v2"
version: "1.0"
subsystem: "chassis"
---

## Overview

The TurtleRabbit SSL robot is designed with a cost-effective, open-source approach using primarily off-the-shelf components and hobby BLDC motors. This document provides a high-level overview of the robot's key subsystems.

## Design Philosophy

Our approach prioritises:

1. **Cost-effectiveness** - Using off-the-shelf components where possible
2. **Accessibility** - Open-source design files and documentation
3. **Manufacturability** - 3D printing and CNC milling for custom parts
4. **Modularity** - Easy to assemble, disassemble, and repair

## Key Subsystems

### Drivetrain

The robot uses a 4-wheel omni-directional drive system with hobby brushless DC motors. This allows movement in any direction without needing to rotate first.

### Kicker

A solenoid-based kicking mechanism for both flat kicks and chip kicks.

### Dribbler

A spinning bar mechanism that allows the robot to control the ball while moving.

### Electronics

- Main controller: Open-source microcontroller board
- Motor drivers: Off-the-shelf ESCs
- Communication: Wireless module for receiving commands from the AI system

### Software Stack

- SSL-Vision for external camera-based localisation
- Custom AI strategy system
- Motion planning and control

## Specifications

| Parameter | Value |
|-----------|-------|
| Diameter | 180mm (max SSL regulation) |
| Height | 150mm (max SSL regulation) |
| Weight | ~2.5 kg |
| Max Speed | ~2 m/s |
| Drive | 4-wheel omnidirectional |
| Motors | Hobby BLDC |

## Related Documents

- [How to Contribute Documentation](/docs/contributing-to-docs/)
