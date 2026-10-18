# VANTIX — Autonomous Robot Car

An Arduino Uno robot car with modular C++ firmware and a custom Flutter controller app. It drives manually over Bluetooth, avoids obstacles on its own, parks itself in a parallel slot, and can record a route and replay it — all without blocking the main loop.

## Features

- **Manual driving** over Bluetooth — 9 speed levels and 8 directions.
- **Autonomous obstacle avoidance** — reads all four directions and picks the clearest path in real time.
- **Automatic parallel parking** — a multi-phase state machine detects a gap with the right-side sensor, overshoots to align the rear, then reverses in with a two-step curve.
- **Teach-and-repeat** — records a sequence of manual commands, then replays the exact route on demand.
- **Drift correction** — pulse-based nudges that compensate for the car's mechanical left-weight bias.
- **Battery monitoring** — voltage read through a divider on `A0` and broadcast to the app.

## Architecture

Firmware is split into focused modules, each with its own `.h` / `.cpp`:

| Module | Responsibility |
|---|---|
| `Motor` | PWM speed + direction for both motors |
| `Sensor` | Four HC-SR04 ultrasonic sensors (front / back / left / right) |
| `Bluetooth` | Command parsing and telemetry |
| `Battery` | Voltage divider reading + periodic broadcast |
| `Modes` | State machine: **Manual / Autonomous / Parking / Teach-and-Repeat** |
| `TeachAndRepeat` | Records and replays command sequences |

A single `Modes` module owns all transitions, so the main loop stays clean and each mode's logic is isolated. The controller is a cross-platform **Flutter** app (speed control, directional input, mode switching, live battery display over Bluetooth).

## Hardware

- Arduino Uno
- 4× HC-SR04 ultrasonic sensors
- Dual DC motors + motor driver
- HC-05 Bluetooth module
- Resistor divider on `A0` for battery sensing

## Build

Open `Final_Code.ino` in the Arduino IDE (or arduino-cli), select the Arduino Uno board, and upload. Pin and threshold values live in `config.h`.

## Team

Ahmed Nagy · Saif Raafat · Omar Wael · Ahmed Morsy · Youssef Moussa · Moataz Hassan
