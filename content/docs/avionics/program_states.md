+++
title = "Program States"
description = "Specification for the avionic system program states."
date = 2023-01-16T08:00:00+00:00
updated = 2023-01-16T08:00:00+00:00
draft = false
weight = 203
sort_by = "weight"
template = "docs/page.html"

[extra]
lead = 'Specification for the avionic system program states.'
toc = true
top = false
+++

## What are Program States?

The activity of launching a rocket is an almost perfect use case for a [state machine](https://en.wikipedia.org/wiki/Finite-state_machine#Usage) conceptualization. The launch of a single stage, solid rocket has well
defined and discrete steps, so it is easy to break the execution of code into states. For example, you do
not expect the rocket to after it has reached apogee and is begining its decent (unless you had a 
re-ignitable and throttlable rocket to use for landing... which this project does not). So you ensure that
no code related to the ignition of the rocket motor is allowed to execute while the rocket is in the 
'decent' stage. At the same time, you do not want the parachute to deploy while the rocket is resting
on the ground.

Even more paramount than the examples mentioned above, is the fact that we **DO NOT** want to initiate the
propulsion system unless we **explicitly** ask it to. And even then, we want to use 
mechanical/electrical/software [interlocks](https://en.wikipedia.org/wiki/Interlock_(engineering)) to 
ensure that the launch sequence will only initiate under a certain state and under certain conditions.
These states and conditions are described below.


## Program States List

The avionics system can be in the following states:

Off, Disabled - There is no power going to any of the rocket systems. Avionics battery is removed or
disconnected.

Off, Connected - There is no power going to any of the rocket systems, but the battery is in place or
connected.

On, Dormant - The rocket is powered on. Sensors powered on, but the micro-controller is not storing
any data. Telemetry is not enabled in this state, no outgoing signals. The rocket is looking for 
incoming signals from the ground station (or debug pins?) instructing it to move on to the next state,
or return to the Off, Connected state. Only these two state signals are accepted at this time.

On, Tx/Rx - The rocket is powered on, sensors powered on, telemetry is active. Some data *may* be 
stored on the eeprom(s). Calibration and initialization is normally done while in this state. The checks 
are done to verify that the rocket is in a condition that it is *eligible* to enter into a **live** state.

On, **live** (grounded) - The rocket is powered on, the Remove-Before-Flight pin has been removed, the sleep
timeout has ended, no abort signal has been sent, and the state flag indicates the rocket is ready
to enter into a **live** state. Once in this state, sensors are running, data is stored as set out in the
initialization setup, and telemetry is transmitted.

On, In-Flight (upwards) - After a signal is sent to the propulsion system and sensors confirm that the
rocket is  moving, the electrical and software interlocks ensure that no more signals can be sent to the
propulsion system. Now, the micro-controller is rapidly trying to store all flight data, transmit all 
flight data, and perform computations to check if flight apogee has been reached, which would bump the
rocket into the next state.

On, In-Flight (downwards) - This is a brief state. All that happens is that a certain amount of time 
passes and the parachute deploys. Once a chute deployment attempt is made (if it is successful or not) the
rocket is bumped to the next state

On, In-Flight (under canopy) - The under canopy bit is assumed, since there is nothing we can do if it 
isn't at this point. The rocket will now prioritize sending GPS coordinates via telemetry over all other
sensor and flight data to help ground crews recover the rocket. 

On, Post-Flight (landed, immobile) - This state is purely for semantics, it changes very little from the
previous state. The rocket will stay in this state until the RBF-pin is replaced in the rocket housing,
or the avionics battery runs low, where the avionics system will eventually shut down. When the RBF-pin 
is placed back in the rocket, it returns to the `On, Tx/Rx` state.


## Propulsion Activation Interlock

Since a lot of energy is released in a violent manor when a rocket motor/engine is fired, we want
to ensure it is only done at a specific time. We also want to use several independent mechanisms to
ensure an accidental firing does not occur. This includes the following mechanisms:

1. Remove-Before-Flight Pin - while the pin is inserted, the rocket is not allowed to be fired. The pin
can be pulled from a safe distance using a long pull-cord. The removal of the pin changes the rocket into 
a **live** state, and a buzzer on the PCB will start to emit a tone. This will alert any person near to 
the rocket that it has become live and if they can hear it, they are probably not a safe distance away. 

2. Software - The primary source of software lockout is the fact that the state machine requires the 
rocket be in a specific state before any ignition code is executed. Secondary to that, if the RBF-pin is
pulled, no code (of any kind) will be allowed to be executed for at least 2 minutes. This allows any 
nearby persons to vacate the area, or for someone to reinsert the pin if it was unintentsionallly 
removed.

3. Electrical - Since ultimately it will be an electrical pulse sent to the propulsion system that 
ignites the motor, care must be taken to ensure that no short circuits can occur, PCB designs must 
ensure lines running to the propulsion system are isolated, and edge case tests should be conducted
to anticipate any faults in the electrical wiring from the avionics PCB to the propulsion system. 
Additionally, a tri-input AND gate can be used, as shown below, to ensure that an output to the 
propulsion system is only sent if (i) the RBF-pin is removed, (ii) The micro-controller program is
in the **live** state as determined by a flag pin output, and (iii) It has been at least 2 minutes 
since the RBF-pin has been removed as determiend by another flag pin.

(insert AND block diagram here.)

Note that many software checks using the sensor data and telemetry will be done before the program
enters the **live** state. These include ensuring that the rocket is not moving and allowing an 
abort signal to be sent at any time, which will disengage any state flags set and prevent the
state flag from being set during the timeout period.
