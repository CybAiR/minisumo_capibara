# minisumo_capibara

## Project Overview
Capibara is an autonomous minisumo class robot designed to compete on a dohyo ring. The robot detects the opponent and pushes them out of the ring while staying inside the white border lines. The system is based on the STM32 Black Pill microcontroller.

## Technologies
* Electronics: KiCad (Project: capibara.kicad_sch)
* Software: C
* Mechanics: Autodesk Inventor (3D printed chassis)

## Folder Structure
- `software` - Source code and firmware
- `hardware` - KiCad schematics, PCB layouts, and components
- `cad` - Autodesk Inventor source files and STL models

## Bill of Materials (BOM)
* MCU: STM32F401CCU6 (Black Pill)
* Motor Driver: TB6612FNG
* Voltage Regulator: L7805 (+5V) with filtering capacitors
* Sensors: 
  * 1x SHARP IR (Opponent detection)
  * 2x QTR Reflectance sensors (Line detection)
* Power: LiPo Battery

## Control Algorithm
The robot operates in a continuous decision loop:
1. Safety: If either the left or right QTR sensor detects the line, the robot reverses and rotates.
2. Attack: If the Sharp sensor detects the opponent, the robot moves forward at full power.
3. Search: If no line or opponent is detected, the robot rotates to scan the ring.

```mermaid
graph TD
    A([Start / Button Pressed]) --> B[5s Safety Delay]
    B --> C{Line Detected?}
    C -- YES --> D[Action: Reverse & Rotate]
    C -- NO --> E{Opponent Detected?}
    E -- NO --> G[Action: Scan / Rotate]
    E -- YES --> F[Action: Charge / Attack]
    D --> C
    F --> C
    G --> C```

## IO Assignment
Pin configuration based on capibara.kicad_sch:

| Pin | Signal | Function |
| :--- | :--- | :--- |
| PA0 | `SIG_SHARP` | Opponent sensor input |
| PA1 | `SIG_QTR_L` | Left line sensor input |
| PA2 | `SIG_QTR_R` | Right line sensor input |
| PA4 | `DIR_L1` | Left Motor Direction 1 |
| PA5 | `DIR_L2` | Left Motor Direction 2 |
| PA6 | `PWM_L` | Left Motor Speed (PWM) |
| PA7 | `MOT_STBY` | Motor Driver Standby |
| PB0 | `SIG_START` | Start button |
| PB5 | `PWM_R` | Right Motor Speed (PWM) |
| PC14 | `DIR_R1` | Right Motor Direction 1 |
| PC15 | `DIR_R2` | Right Motor Direction 2 |

## Commit Conventions
- `hw:` CAD and hardware development.
- `docs:` Documentation updates.
- `feat:` New features.
- `fix:` Bug fixes.
- `wip:` Work in progress.
