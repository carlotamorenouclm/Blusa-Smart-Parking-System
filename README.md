# BLUSA — Smart Parking Management System

**An ESP32-S3-based prototype for automated parking access, NFC authentication, occupancy tracking, and Wi-Fi monitoring.**

> **Project status:** In development. This README describes the planned prototype; features and connections will be updated as they are implemented and tested.

## Overview

BLUSA is an educational embedded-systems project built around the **LOLIN S3 development board (ESP32-S3)**. It uses two separate access lanes—one for entry and one for exit—to manage parking occupancy and provide information about available spaces.

At the entrance, users present an authorized NFC card before the barrier can open. At the exit, the barrier opens when a vehicle is detected. Ultrasonic sensors monitor the approach and passage of vehicles, and the controller updates the occupancy counter after a confirmed movement.

The ESP32-S3's built-in Wi-Fi will make parking information available through a remote monitoring interface.

## Planned features

- **NFC entry authentication** using a PN532 reader and compatible cards.
- **Two independent automatic barriers**, controlled by servo motors.
- **Vehicle detection** using four HC-SR04 ultrasonic sensors (two per lane).
- **Occupancy tracking** based on confirmed entries and exits.
- **Capacity control** to reject new entries when the parking area is full.
- **Local status display** using an LCD or OLED screen (model to be selected).
- **Audible feedback** using a buzzer.
- **Remote status monitoring** over the ESP32-S3's integrated Wi-Fi.

## How it is intended to work

### Entry lane

1. The first ultrasonic sensor detects an approaching vehicle.
2. The driver presents an NFC card to the PN532 reader.
3. The controller checks card authorization and parking availability.
4. If access is permitted, the entrance barrier opens.
5. The second ultrasonic sensor helps confirm vehicle passage.
6. Once the passage is complete and the lane is clear, the barrier closes and occupancy is updated.

### Exit lane

1. The third ultrasonic sensor detects a vehicle approaching the exit.
2. The exit barrier opens automatically.
3. The fourth ultrasonic sensor helps confirm passage.
4. Once the passage is complete and the lane is clear, the barrier closes and occupancy is updated.

### Occupancy calculation

```text
Available spaces = Total capacity - Occupied spaces
```

The planned software will register at most one entry or exit per completed access cycle and keep occupancy between zero and the configured maximum capacity.

## Hardware

| Component | Quantity | Purpose |
|---|---:|---|
| LOLIN S3 (ESP32-S3) | 1 | Main controller and built-in Wi-Fi |
| PN532 NFC reader | 1 | Entrance authentication |
| NFC cards/tags | As needed | User identification |
| HC-SR04 ultrasonic sensors | 4 | Approach and passage detection |
| Servo motors | 2 | Entrance and exit barriers |
| LCD or OLED display | 1 | Local system status |
| Buzzer | 1 | Audible notifications |
| Regulated power supply | As needed | Power for the board and peripherals |
| Jumper wires, resistors, prototyping materials | As needed | Assembly and electrical interfacing |

**Electrical considerations:** ESP32-S3 GPIO uses **3.3 V logic**. Standard 5 V HC-SR04 ECHO outputs must be level-shifted or divided to a safe input level. Servo motors should be powered from a suitable supply rather than directly from a GPIO pin, with a common ground between their supply and the controller. Final wiring and power requirements depend on the exact module variants.

## Software architecture (planned)

The firmware will organize each lane as a state machine, with states for vehicle detection, authorization where applicable, barrier operation, passage verification, and occupancy updates. Shared occupancy data will be presented on the local display and through Wi-Fi.

Implementation choices—including the programming framework, PN532 interface (I2C/SPI/UART), display model, GPIO assignments, Wi-Fi interface, and counter persistence—will be documented once finalized.

## Getting started

The hardware design and firmware are still being developed. A verified wiring diagram, setup instructions, required libraries or dependencies, build and upload steps, and configuration guidance will be added after the first working implementation.

**Do not connect 5 V signals directly to ESP32-S3 GPIO pins.** Check the actual voltage and current requirements of each component before powering the prototype.

## Testing plan

- Authorized and unauthorized NFC card checks.
- Vehicle approach and passage detection in both lanes.
- Independent entrance and exit barrier operation.
- One-count-per-vehicle occupancy updates.
- Full-capacity entry rejection while preserving exit operation.
- Local display, buzzer, and Wi-Fi status updates.
- Recovery behavior after interrupted movements or controller resets.

This is a **demonstration prototype**, not a safety-certified barrier controller. Ultrasonic detection alone is not sufficient for real-world vehicle or pedestrian safety.

## Documentation

The seminar report will cover the system overview, hardware components, software development, implementation, tests, and results. Diagrams and photos can be added to this repository as the project progresses.

