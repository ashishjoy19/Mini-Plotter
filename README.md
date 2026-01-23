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

# ⚡ ESP32-WROOM-32E – PCB Design & Boot Guidelines

This document provides **electronics design guidelines** for using the **ESP32-WROOM-32E** module in custom PCBs.

It focuses on:
- Boot and flashing requirements
- Strapping pins and default states
- Pins to avoid or use with caution
- Minimum required external circuitry
- PCB layout considerations

---

## ESP32-WROOM-32E Overview

- MCU: ESP32 (dual-core Xtensa LX6)
- Operating Voltage: **3.3V**
- Wi-Fi + Bluetooth on-module
- Integrated antenna (keep-out required)

⚠️ **All GPIOs are 3.3V only. Do NOT apply 5V.**

---

## Minimum Circuit for Boot & Flash


::contentReference[oaicite:0]{index=0}


### Mandatory Connections

| Pin | Requirement |
|----|------------|
| 3V3 | Stable 3.3V supply (≥ 600 mA peak) |
| GND | Solid ground connection |
| EN | Pulled HIGH (via 10kΩ) |
| IO0 | Pulled HIGH (via 10kΩ) |
| TX0 (GPIO1) | UART TX for flashing |
| RX0 (GPIO3) | UART RX for flashing |

---

## Boot & Flash Logic

### Boot Mode Selection

| IO0 State | Boot Mode |
|---------|-----------|
| HIGH | Normal boot (flash) |
| LOW | UART download (flashing) |

To flash firmware:
- **IO0 = LOW**
- **EN toggled LOW → HIGH (reset)**

---

## Strapping Pins (CRITICAL)

These pins are sampled **at reset**.  
Incorrect levels will prevent booting.

| GPIO | Default State | Function |
|----|--------------|---------|
| GPIO0 | Pull-up | Boot mode select |
| GPIO2 | Pull-down | Must be LOW at boot |
| GPIO4 | Pull-down | Boot config |
| GPIO5 | Pull-up | Boot config |
| GPIO12 | Pull-down | Flash voltage select |
| GPIO15 | Pull-up | Boot config |

⚠️ **Do not drive these pins externally at boot unless you know the impact.**

---

## Pin Usage Guidelines

### Pins to Avoid (or Use Carefully)

| GPIO | Reason |
|----|-------|
| GPIO6–11 | Connected to internal flash (DO NOT USE) |
| GPIO0 | Boot mode pin |
| GPIO2 | Must be LOW during boot |
| GPIO12 | Affects flash voltage |
| GPIO15 | Boot strapping pin |

---

### Recommended Safe GPIOs

| GPIO | Notes |
|----|------|
| GPIO4 | Safe after boot |
| GPIO5 | Safe after boot |
| GPIO16–19 | General purpose |
| GPIO21–23 | I²C / SPI friendly |
| GPIO25–27 | ADC capable |
| GPIO32–33 | ADC + low power |
| GPIO34–39 | Input-only (ADC) |

---

## Pull-Up / Pull-Down Requirements

### Required External Resistors

| Pin | Recommended |
|----|-------------|
| EN | 10kΩ pull-up to 3.3V |
| IO0 | 10kΩ pull-up |
| GPIO2 | 10kΩ pull-down |
| GPIO12 | 10kΩ pull-down |
| GPIO15 | 10kΩ pull-up |

⚠️ Internal pulls exist but **external resistors improve reliability**.

---

## Reset & Auto-Flash Circuit (Recommended)

For USB-UART auto programming:

| ESP32 Pin | USB-UART Signal |
|--------|----------------|
| EN | DTR (via transistor) |
| IO0 | RTS (via transistor) |
| RX0 | TXD |
| TX0 | RXD |

This allows flashing via:
- Arduino IDE
- ESP-IDF
- PlatformIO

---

## Power Supply Design Notes

- Use **LDO or buck regulator** capable of **600–800 mA**
- Place **10µF + 0.1µF decoupling capacitors** close to 3V3 pin
- Keep power traces wide and short

⚠️ Brownouts are the #1 cause of ESP32 instability.

---

## Antenna & RF Layout Rules

⚠️ **Very Important**

- Keep copper **clear under antenna**
- No ground pours or traces under antenna area
- Follow Espressif keep-out recommendations
- Place module at PCB edge if possible

---

## Common Design Mistakes

❌ Powering ESP32 from 5V  
❌ Using GPIO6–11  
❌ Driving strapping pins at boot  
❌ Weak 3.3V regulator  
❌ Traces under antenna  

---

## Recommended Bring-Up Checklist

1. Verify 3.3V rail (no load)
2. Check EN pulled HIGH
3. Check IO0 pulled HIGH
4. Reset → observe serial output
5. Pull IO0 LOW → reset → flash test firmware

---

## Reference

- ESP32-WROOM-32E Datasheet
- Espressif Hardware Design Guidelines

---

## Notes

This document is intended for **PCB designers and electronics engineers** working with ESP32-WROOM-32E in custom hardware.

Keep this file updated if pin usage or constraints change.
