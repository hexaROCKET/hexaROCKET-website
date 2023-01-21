+++
title = "Sensors"
description = "Avionics sensor specifications and documents."
date = 2023-01-16T08:00:00+00:00
updated = 2023-01-16T08:00:00+00:00
draft = false
weight = 204
sort_by = "weight"
template = "docs/page.html"

[extra]
lead = 'Avionic sensor specifications and documents'
toc = true
top = false
+++

## Sensor List

| Component        | Name      | Bus Protocol |
|------------------|-----------|--------------|
| Altimeter        | BME280    | SPI          | 
| IMU              | BNO055    | I2C          |
| GPS Module       | M20048-1  | UART         |

## Altimeter

**[Datasheet](../bme280.pdf/)**

Although the sensor datasheet does not say that the BME280 is an altimeter, it collects all the pertinent 
information to get [pressure altitude](https://en.wikipedia.org/wiki/Pressure_altitude), which we will be
using to confirm if we have met our 3,000 metre altitude goal. We can either use the NOAA formula to 
directly convert from atmospheric pressure to altitude. However, this formula assumes a 
[standard temperature](https://en.wikipedia.org/wiki/Standard_temperature_and_pressure). If we know the
actual temperature at the altitude we are flying at, we can also use the 
[Hypsometric Equation](https://keisan.casio.com/exec/system/1224585971) to determine the altitude above 
sea level.

### Crash Course: How an Altimeter Works

stuff...

### BME280: Operational Overview

The BME280 will be connected to the RP2040 via an SPI bus. The simplest way to get the required 
measurements is to use the [bme280 driver crate](https://crates.io/crates/bme280). However, the 
measurement function provides the temperature, humidity and pressure values as floating point 
numbers. The problem is that the RP2040 does not have an 
[FPU](https://en.wikipedia.org/wiki/Floating-point_unit), so there may become issues down the 
line where the float computations are taking up too much of the RP2040 CPUs clock cycles. 

## Inertial Measurement Unit

**[Datasheet](../bno055.pdf/)**

The IMU is not strictly necessary to meet the [goals](../../project_overview/goals/) of 
this project. However, the IMU provides valuable engineering insights that are useful for
determining the performance of the propulsion system and iterating on the design of the 
airframe.



### Crash Course: How an IMU Works

stuff...

### BNO055: Operational Overview

[bno055 driver crate](https://crates.io/crates/bno055)

[quaternion](https://en.wikipedia.org/wiki/Quaternion)

## GPS Module

**[Datasheet](../m20048-1.pdf/)**

The GPS module is critical for recovering the rocket after the flight. It can also be used to back
up the claims made by the altimeter sensor.

### Crash Course: How GPS Works

stuff...

### M20048-1: Operational Overview