# Electronics 

# ⚡ Mini Plotter V2 – Electronics Documentation

## Overview

This document covers **the electronics subsystem** of the Mini Plotter V project.

It is intended as a **reference for coworkers, fabrication, debugging, and replication** of the electronics and PCB.

---

## Scope

### Included
- PCB design reference
- Power architecture
- Microcontroller pin mapping
- Stepper driver connections
- Peripheral interfaces

**Board Details**

| Parameter | Value |
|--------|------|
| PCB Name | Mini Plotter V2 – Electronics |
| Layers | 2 |
| Thickness | 1.6 mm |
| Copper Weight | 1 oz |
| Assembly | Through-hole + SMD |
| Logic Voltage | 3.3V |
| Motor Voltage | 5V DC |

## Power Architecture

### Power Input
- External DC input - USB C (Power only)
- Recommended supply: **5V**

### Voltage Rails
- **Motor Rail:** Direct from input to stepper drivers
- **Logic Rail:** Regulated 3.3V for MCU and peripherals

### Electrical Notes

⚠️ **Important**
- Power supply must handle motor current peaks
- Logic and motor grounds are common
- Do not connect or disconnect motors while powered

---

## MCU Pin Mapping

## GPIO Pin Mapping (Grouped)

### Motor 1 (Stepper)
| Label | GPIO |
|-----|------|
| IN1 | GPIO 23 |
| IN2 | GPIO 22 |
| IN3 | GPIO 21 |
| IN4 | GPIO 19 |

### Motor 2 (Stepper)
| Label | GPIO |
|-----|------|
| IN5 | GPIO 18 |
| IN6 | GPIO 17 |
| IN7 | GPIO 16 |
| IN8 | GPIO 4 |

### Servo Motor
| Label | GPIO |
|------|------|
| SIGNAL | GPIO 13 |
| VCC | 5V |
| GND | GND |

### Limit Switches
| Switch | Signal GPIO | Ground |
|-------|------------|--------|
| X Limit | GPIO 27 | GND |
| Y Limit | GPIO 14 | GND |

---

