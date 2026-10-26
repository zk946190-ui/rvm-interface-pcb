# Reverse Vending Machine Interface PCB (Rev 1)

KiCad schematic and PCB layout for an interface board that connects an **Arduino Mega** to the sensors, actuators and communication modules of a Reverse Vending Machine (RVM). The board replaces breadboard and loose-wire connections with one organised PCB.

**Status:** Rev 1 design, **not fabricated and not tested on hardware yet.** See [Progress](#progress).

## Schematic

![RVM interface schematic](images/schematic.png)

## PCB layout

![RVM interface PCB layout, Rev 1](images/pcb-layout.png)

The board is routed on the top and bottom copper layers, with four M3 mounting holes (3.2 mm).

## Project objectives

- Capture the circuit as a KiCad schematic.
- Lay out and route a PCB that connects all RVM modules to the Arduino Mega through one header.
- Keep signal, power and ground connections organised and labelled.
- Use connectors for every module so the board can be wired and replaced easily.
- Prepare the design for design-rule checks and fabrication.

## Modules and connectors

| Module | Connector on board | Power rail |
|---|---|---|
| Arduino Mega | Dual-row header (MEGA_LINK) | n/a |
| LCD (I2C) | LCD header (SDA, SCL) | 5V_MEGA |
| HX711 load cell amplifier | HX711 header (HX_DT, HX_SCK) | 5V_MEGA |
| Metal sensor | METAL_SENSOR header | 5V_MEGA |
| Bluetooth (HC-05) | BLUETOOTH header | 5V_MEGA |
| 4x4 keypad | KEYPAD header (4 rows, 4 columns) | n/a |
| Servo motor (select) | SERVO_SELECT header | 5V_EXT |
| Servo motor (drop) | SERVO_DROP header | 5V_EXT |
| GSM module | GSM header (GSM_TX, GSM_RX) | 5V_EXT |
| Buzzer | BUZZER header (driven by a transistor) | 5V_EXT |
| External power input | POWER_IN screw terminal | 5V_EXT |

## Circuit design notes

- **Two supply rails:** the logic modules (LCD, HX711, Bluetooth, metal sensor) use `5V_MEGA`. The higher-current loads (servos, GSM, buzzer) use a separate external `5V_EXT` supply. All grounds are on one common `GND` net.
- **Bluetooth RX level divider:** a 1 kΩ and a 2.2 kΩ resistor form a voltage divider on the HC-05 RX line, which is fed from the Arduino TX1 pin.
- **Buzzer driver:** a BC547 NPN transistor with a 1 kΩ base resistor switches the buzzer from the `BUZ_CTRL` signal. A 1N4148 diode is placed across the buzzer.
- **Power input:** a 1N5819 Schottky diode sits in the external 5 V input path.
- **Decoupling:** a 0.1 µF capacitor and a 1000 µF capacitor sit on the `5V_EXT` rail.
- **Indicator:** an LED with a 330 Ω resistor on `5V_EXT`.

## Signal names

Fill in the Arduino pin used for each signal.

| Signal | Function | Arduino Mega pin |
|---|---|---|
| SDA, SCL | LCD (I2C) | ___ , ___ |
| HX_DT, HX_SCK | HX711 load cell amplifier | ___ , ___ |
| METAL | Metal sensor output | ___ |
| SERVO_SEL, SERVO_DROP | Servo control signals | ___ , ___ |
| MEGA_TX1 / BT_TXD | Bluetooth link | ___ |
| GSM_TX, GSM_RX | GSM module serial | ___ , ___ |
| BUZ_CTRL | Buzzer control | ___ |
| KEY_R1 to KEY_R4 | Keypad rows | ___ |
| KEY_C1 to KEY_C4 | Keypad columns | ___ |

## Project files

| File | Description |
|---|---|
| `rvm interface pcb.kicad_pro` | KiCad project |
| `rvm interface pcb.kicad_sch` | Schematic |
| `rvm interface pcb.kicad_pcb` | PCB layout |

Open the `.kicad_pro` file with KiCad (the project was created with KiCad 10).

## Progress

- [x] Schematic capture
- [x] Component placement and track routing (Rev 1)
- [ ] Electrical Rules Check (ERC) passed
- [ ] Design Rules Check (DRC) passed
- [ ] 3D view and Bill of Materials (BOM)
- [ ] Gerber and drill files
- [ ] Fabrication and hardware testing

## What I learned

- Drawing a schematic and assigning component footprints in KiCad
- Placing components and routing tracks on a two-layer board
- Designing separate power rails and a level divider for 3.3 V and 5 V modules
- Preparing a design for design-rule checks and manufacturing

## Disclaimer

This is an educational PCB design project based on a Reverse Vending Machine concept. The board has not been fabricated or validated on physical hardware. Check the connections, footprints, power requirements and design rules against each module's datasheet before building it.

## Related project

[RVM Verification Framework](https://github.com/zk946190-ui/rvm-verification-framework): a Python test framework and REST API for a simulated version of the same machine.
