+++
title = "Overview"
description = "An overview of the Avionics System."
date = 2023-01-16T08:00:00+00:00
updated = 2023-01-16T08:00:00+00:00
draft = false
weight = 201
sort_by = "weight"
template = "docs/page.html"

[extra]
lead = 'An overview of the Avionics System.'
toc = true
top = false
+++

## Introduction

As mentioned in the [project overview](../../project_overview/introduction/), the main motivation of this 
project is to learn. As such, the question is, what do we want to learn? Below is a list of tools that
we would like to gain more experience with:

1. Embedded Rust, [Hardware Abstraction Layer library](https://github.com/rust-embedded/embedded-hal)
    - Using the Rust Embedded HAL, we can very easily swap in and swap out PCB components, and we are
    less tied down to a specific micro-controller/sensor/component down the line. In short, the HAL 
    allows us more flexibility in the design and Rust enables us to write safe programs, nice programs, and doing so in a modern build system.
    - [Awesome Embedded](https://github.com/rust-embedded/awesome-embedded-rust) can be used to 'shop' 
    around and find interchangable components to add to our avionics system.

2. [KiCad](https://www.kicad.org/), an open source schematic design program that enables us to design and
layout our PCB with all of the avionic components.

3. It may also be interesting to use a home CNC router to rapidly develop PCB boards. To convert our KiCad PCBs into a physical PCB, we will need to generate the G-code to run on the router. [FreeCAD](https://www.freecad.org/)
Is the open source tool that can do this for us.

So now we know what language we want to use, where we can find components that will work together in 
our build, we have a means of developing a PCB layout, and a means of bringing that PCB into the physical
world using exclusively **open source** tools. Using **open source** tools is one of the most important
pillars of this build. We would like to avoid proprietary tools at all costs.

<br>

## System Hardware

The following design principles are what will guide our selection of hardware components:

1. Non-obsolete - Use modern components that are not considered end-of-life.

2. Widely adopted - Use components that will be easier to troubleshoot via search engines.

3. Low cost - Rather than use one component that "does it all", prefer using multiple components
that are a fraction of the cost.

<br>

Following the design principles above, the following components have been considered:

| Component        | Name      | Bus Protocol | Price/unit |
|------------------|-----------|--------------| -----------|
| Micro-controller | RP2040    | n/a          | $2.00      |
| Altimeter        | BME280    | SPI          | $10.47     |
| IMU              | BNO055    | I2C          | $22.07     |
| GPS Module       | M20048-1  | UART         | $23.76     |
| Tx/Rx            | SX1280    | SPI          | $9.07      |
| EEPROM           | 24LC256   | I2C          | $1.74      |


total GPIO pins needed: 4 + 2 + 4 + 4 + 2 = 16

These components were not choosen at random. Below is the rationale for each component:

1. Micro-controller - Raspberry Pi RP2040
    - Excellent documentation in the HAL library
    - The price is right (~$4)
    - Many GPIO pins available to use.

<br>

2. Altimeter - Bosch BME280
    - HAL driver available
    - Not obsolete

<br>

3. IMU (Inertial Measurement Unit) - Bosch BNO055
    - HAL driver available
    - Not obsolete

<br>

4. GPS Module - Antenova M20048-1
    - Provides a standardized output (no driver available)
    - Integrated Antenna
    - Low cost

<br>

5. Tx/Rx (LoRaWAN Module) - Semtech SX1280
    - HAL Driver Available
    - Low Cost
    - (-) Gotta use an external antenna

<br>

6. EEPROM - 24LC256
    - Fairly standard component

## System Software


The Hardware Abstraction Library is going to be the very powerful glue that puts the
entire avionics system together. As such, below are some resources on our toolchain:

- [Rust HAL library](https://github.com/rust-embedded/embedded-hal)
- [RP2040 HAL library](https://github.com/rp-rs/rp-hal)

