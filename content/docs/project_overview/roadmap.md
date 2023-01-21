+++
title = "Project Roadmap and Milestones"
description = "An overview of what the hexaROCKET project is trying to acheive by looking our incremental milestones."
date = 2021-05-01T08:00:00+00:00
updated = 2021-05-01T08:00:00+00:00
draft = false
weight = 103
sort_by = "weight"
template = "docs/page.html"

[extra]
lead = 'An overview of what the hexaROCKET project is trying to acheive by looking our incremental milestones.'
toc = true
top = false
+++

## Overview

One page summary of how to start a new AdiDoks project. [Quick Start →](../quick-start/)

## Roadmap

1. Develop Design Outline for the proposed sensors, controllers, etc. for the Avionics PCB.
2. Determine Program States for a solid propellant, single stage rocket.
3. Develop data storage protocol.
4. Design PCB layout and connections for all avionics components, taking into consideration
the communication protocols used for each embedded component.
5. Develop test suite layout for all avionic software (we are not a fast-and-loose development group).
6. Rough out software functions and modules. Only module names, function names/parameters/returns/errors 
at this point.
6. Perform unit tests on all embedded hardware components.
7. Implement data storage system using altimeter data as first test sensor, implement all other sensor data
after altimeter data is properly collected/stored/retrieved using the EEPROM (no telemetry at this point).
8. Choose and test telemetry system using basic, non-avionic-system data
9. Develop ground station, including data input, storage, and live display.
10. Completed telemetry build-out for all program states.

Notice that we are at 10 steps so far, and we haven't even begun to build the rocket.

11. Establish a list of possible rocket motors or propellants that are candidates for our 3,000 metre
altitude target.
12. Estimate the compressive (or other) forces that our airframe will need to withstand for the given 
short-list of propulsion systems.
13. Propose materials and constructions methods that will withstand the anticipated forces.
14. Build a scale model rocket to test the complete avionics package at low altitudes.
15. Launch and recover scale model rocket. Iterate systems as needed. Critically, ensure that 
mechanical/electrical/software lockouts work as expected.
16. Build scaled version of propulsion system and perform bench tests. Iterate as needed.
17. Build full sized rocket airframe using trial-and-error methods.
18. Build full sized propulsion system and perform bench test. Iterate as needed.
19. Replicate full sized propulsion system for final build.
20. Build launch tower assembly.
21. Assemble all final rocket components.
22. Attempt to launch rocket to 3,000 metres.


## Milestones

The project can be broken up into the following phases (delineated by major milestones):

A. Avionics, without telemetry

B. Avionics, with telemetry and ground station

C. Scale model launch completed

D. Scale model propulsion bench test completed

E. Full sized airframe completed

F. Full integration of all systems

G. First test flight of all full sized systems completed

H. 3,000 metres altitude reached with full recovery
