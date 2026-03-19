---
title: "How to Contribute Documentation"
date: 2025-01-01T00:00:00+11:00
description: "Guide for team members on how to add and maintain documentation for TurtleRabbit robots."
category: "general"
author: "TurtleRabbit Team"
version: "1.0"
---

## Overview

All TurtleRabbit documentation is stored as Markdown files in the `content/docs/` directory of our GitHub repository. Anyone can contribute by creating a new file and submitting a pull request.

## How to Add a New Document

### Step 1: Create a New File

Create a new `.md` file in the `content/docs/` directory. Name it using lowercase with hyphens (e.g., `motor-controller-setup.md`).

### Step 2: Add Frontmatter

Every document must start with YAML frontmatter. Here are the required and optional fields:

```yaml
---
title: "Your Document Title"          # Required
date: 2025-03-15T00:00:00+11:00      # Required - use current date
description: "Brief summary"          # Required - shown in doc cards
category: "mechanical"                # Required - see categories below
author: "Your Name"                   # Required
robot: "v2"                           # Optional - which robot version
version: "1.0"                        # Optional - document version
subsystem: "drivetrain"               # Optional - specific subsystem
draft: false                          # Set true to hide from listing
---
```

### Step 3: Write Content

Use standard Markdown for your content. Include:

- Clear headings with `##` and `###`
- Code blocks for any commands or code
- Images (place them in `/static/images/docs/`)
- Tables for specifications or comparisons
- Links to related documents

### Step 4: Submit a Pull Request

Commit your file and open a pull request to the `main` branch. A team member will review and merge it.

## Categories

Use one of these categories in your frontmatter:

| Category | Description |
|----------|-------------|
| `mechanical` | Chassis, drivetrain, kicker, dribbler, 3D printing |
| `electrical` | PCB design, wiring, power systems, sensors |
| `software` | Strategy, motion planning, AI, game logic |
| `firmware` | Microcontroller code, motor control, communication |
| `vision` | Camera systems, SSL-Vision, ball/robot detection |
| `general` | Setup guides, team processes, contributing |

## Best Practices

- Keep titles concise and descriptive
- Include a brief overview at the top of each document
- Add images and diagrams where helpful
- Link to related documentation
- Update the `date` field when making significant changes
- Use the `version` field to track major revisions
