---
title: "Soccer training"
date: 2024-02-12T17:34:23+11:00
featureImage: images/blog/IMG_5281.jpeg
tags: [ "AI", "Software"]
---

The software that runs locally on each robot translates requests for a robot velocity into individual motor velocities. From a centralised team controller, a computer outside the field, our robots receive the requested robot velocities, calculates the appropriate motor velocities using the geometry of the robot, and passes these resulting values to the individual controllers via a CAN bus interface. Low level control is handled by each motor controller and their firmware. Our python module handles communication with the centralised team controller that implements the team strategy.