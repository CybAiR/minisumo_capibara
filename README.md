# minisumo_capibara

Documentation for an autonomous minisumo class robot based on the STM32 architecture.

## Project Overview
**Capibara** is a minisumo class robot (dimensions 10x10 cm, weight <500g) designed for autonomous combat on a dohyo ring. The robot's primary objectives are to detect the opponent and push them out of the ring while remaining within the white border lines. The brain of the robot is an **STM32 Black Pill** microcontroller.

---

## Technologies
* **Electronics:** KiCad 10.0.0 (Project: `capibara.kicad_sch`)
* **Software:** C (Logic based on a Finite State Machine)
* **Mechanics:** Autodesk Inventor / Fusion 360 (3D Printing)
* **Version Control:** Git / GitHub Flow using specific commit conventions

---

## Folder Structure
This repository is organized to separate different engineering domains:

- 📁 `software` – Source code and firmware
- 📁 `hardware` – KiCad schematics
- 📁 `cad` – Mechanical documentation, including Inventor source files and `.stl` printables

---

## Bill of Materials
Key components as defined in the `hardware` schematic:
* **MCU:** STM32F401CCU6 (Black Pill)
* **Motor Driver:** TB6612FNG (Dual H-Bridge)
* **Voltage Regulator:** L7805 (+5V) with 100uF and 10uF filtration
* **Sensors:** 
  * 1x SHARP IR (Opponent detection)
  * 2x QTR Reflectance sensors (Line detection)
* **Power:** LiPo Battery (Input via `+BATT`)

---

## Control Algorithm
The robot operates using a prioritized decision loop:
1. **Safety First:** If a QTR sensor detects the white line (`SIG_QTR_L` or `SIG_QTR_R`) -> Immediate reverse and rotation.
2. **Attack:** If the Sharp sensor (`SIG_SHARP`) detects a target -> Full power forward.
3. **Search:** If no line and no opponent are detected -> Rotate to scan the surroundings.

### Decision Logic Flowchart
```mermaid
graph TD
    A([Start / Button Pressed]) --> B[5s Safety Delay]
    B --> C{Line Detected?}
    C -- YES --> D[Action: Reverse & Rotate]
    C -- NO --> E{Opponent Detected?}
    E -- YES --> F[Action: Charge / Attack]
    E -- NO --> G[Action: Scan / Rotate]
    D --> C
    F --> C
    G --> C
